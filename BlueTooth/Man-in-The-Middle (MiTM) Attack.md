## Man-in-The-Middle (MiTM) Attack
Aim: Intercepting the Bluetooth LE traffic between Central device and Peripheral Device.
Follow below steps to set up the environment:
* Install DVPD Android application in mobile and login using credentials (admin/12345), will be known as Central Device.

![image](https://user-images.githubusercontent.com/48615614/158756593-0f4467af-ee6f-407a-a94a-b9f3787474f7.png)

* Run dvpd.js in Raspberry Pi (one out of 2 Raspberry Pi), will be known as Smart Device or Peripheral Device.

![image](https://user-images.githubusercontent.com/48615614/158756688-a5b5abd8-f06e-4a88-8e69-700db1ee911d.png)

* Connect to DVPD peripheral using DVPD mobile app

![image](https://user-images.githubusercontent.com/48615614/158756859-bd4738dc-62bb-4145-8964-0212554b1ab5.png)

* Verify the communication between DVPD mobile app and peripheral dvpd.js application

![image](https://user-images.githubusercontent.com/48615614/158756919-faf4efb3-9c68-4331-9b43-bfb4f2566cc4.png)

Figure: DVPD Peripheral and mobile app communication (Turning ON and OFF bulb)

![image](https://user-images.githubusercontent.com/48615614/158757575-b5ba5667-b04f-4e5a-ba7b-b09adac152a9.png)

Figure: DVPD Peripheral and Central device communication (Changing bulb colors)

### In the current set up environment, MiTM Attack Scenario is the following:
* 1st Raspberry Pi 3: Smart Device aka Peripheral Device
* Android Phone: Smart Mobile App aka Client, also known as Central Device
* 2nd Raspberry Pi 3: Client Clone
* Kali Linux OS: Smart Device Clone (running on VM with BLE adapter)

![image](https://user-images.githubusercontent.com/48615614/158757826-8f9c5a50-6686-4c2b-abb2-bd1b4d302473.png)

![image](https://user-images.githubusercontent.com/48615614/158757922-6f0f6a63-ff13-4ac1-813d-987826d65362.png)

Step 1: Check for BLE adapter and UP the interface
* `hciconfig`
* `hciconfig hci0 up`
* `hciconfig hci0 version` (Check for BLE support)

![image](https://user-images.githubusercontent.com/48615614/158758110-e46824ab-da5a-4f7d-b4fe-0e65bf761cb3.png)

Step 2: Edit Configurations, follow the below phases to edit the configuration file on Kali Linux:

Phase 1: Edit config file to Raspberry Pi (on Kali)
```
gedit config.env
Edit BLENO_HCI_DEVICE_ID to your HCI (e.g hci0 interface), WS_SLAVE address to match your Raspberry Pi. example:
#"peripheral" device emulator
BLENO_HCI_DEVICE_ID=0
# ws-slave websocket address
WS_SLAVE=127.0.0.1 modify to WS_SLAVE=RaspberryPi_IP
```
Phase 2: MAC address spoofing (if mobile applications rely on MAC address)
```
> cd ~/node_modules/gattacker/helpers/bdaddr
Bluetooth LE Hacking - Understanding Bluetooth LE communication and demonstrating Man-in-The-Middle attack on Bluetooth LE
> make
> gcc -c bdaddr.c
> gcc -c out.c
> gcc -o bdaddr bdaddr.o out.o -lbluetooth
> cp bdaddr /usr/local/sbin
Note: bdaddr tool is used by Gattacker to spoof MAC address
```

Step 3: Scan for devices
Phase 1: Running ws-slave on Raspberry Pi

`sudo node ws-slave.js`

![image](https://user-images.githubusercontent.com/48615614/158759100-a11b1d6e-a524-4ff3-8724-485b390b0f84.png)

Figure: Running ws-slave on Raspberry Pi

Phase 2: Scan for advertisements on Kali
`node scan.js` (without parameter, it will listen for all advertisements)

![image](https://user-images.githubusercontent.com/48615614/158760304-2bf47e57-f10c-4c19-aab2-074cdb349905.png)

Figure: Running scan for advertisements on Kali

![image](https://user-images.githubusercontent.com/48615614/158760356-848d843d-6085-4bed-84a8-3e579717ad08.png)

Figure: BLE traffic passing through Raspberry Pi (Client)

Phase 3: Scan target device for Characteristics
All the advertisements and services will be stored in `json` format inside `devices/` directory.
`node scan.js <target_mac_address>`

![image](https://user-images.githubusercontent.com/48615614/158760536-b29e5434-6bb0-4f02-8089-fe2d7c27fc81.png)

Figure: Running scan for specific target on Kali Linux

![image](https://user-images.githubusercontent.com/48615614/158760684-1eec28bd-bfc5-43da-a029-4685fa5e7d61.png)

Figure: BLE traffic from Kali Linux through Raspberry Pi

Step 4: Advertise
Once all the advertisements and services are collected from Original Peripheral, Kali Linux will be used to advertise

Same packets but with a faster rate than the original peripheral device. When the central device receives the first
packets from clone device, it starts interpreting and considered as intended device.
`./mac_adv -a devices/<peripheral_advertisements>.adv.json -s devices/<target_services>.srv.json`

![image](https://user-images.githubusercontent.com/48615614/158760929-c5eb75c5-d88a-4673-a470-0c9a83f1baaf.png)

Figure: Advertisements from Kali Linux

Re-plug the adapter after changing MAC address.

![image](https://user-images.githubusercontent.com/48615614/158761003-d9de4cd0-07b8-41a2-9983-0dc3830a4997.png)

Figure: Kali Linux as Smart Peripheral Device and waiting for connection

Step 5: Disconnect the Mobile app and again scan for BLE devices

![image](https://user-images.githubusercontent.com/48615614/158761087-32d99bda-9503-489f-b607-22e54151154a.png)

Figure: DVPD App Scanning for BLE device

Step 6: Active Eavesdropping
Once the central device initiates connection, all the BLE communication intended for the peripheral device can be seen on screen.

![image](https://user-images.githubusercontent.com/48615614/158761433-17340eb8-cab8-45cf-a7ff-c0427ac11017.png)

Figure: BLE traffic from/to Central device to/from Peripheral

All the BLE communication intended for the original peripheral from the central device will be accepted by Kali Linux (Smart Device Clone) and sent to Raspberry Pi (Client Clone), then from Raspberry Pi it will reach the original peripheral device, and vice-versa.

![image](https://user-images.githubusercontent.com/48615614/158761513-8f6d95bd-48ba-4421-8f5f-3e2020941901.png)

Figure: MiTM Attack - Command line view

## References:
* https://en.wikipedia.org/wiki/Bluetooth_Low_Energy 
* http://www.gattack.io/# 
* https://eewiki.net/display/Wireless/A+Basic+Introduction+to+BLE+Security 
* https://conference.hitb.org/hitbsecconf2017ams/materials/D2T3%20-%20Slawomir%20Jasek%20-%20Blue%20Picking%20-%20Hacking%20Bluetooth%20Smart%20Locks.pdf 
* https://github.com/ji2kumar/dvpd 
* https://github.com/0ddblade/dvpd 
* https://github.com/sandeepmistry/noble 
* https://github.com/sandeepmistry/bleno 
* http://www.smartlockpicking.com/hackmelock/ 

















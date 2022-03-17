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






























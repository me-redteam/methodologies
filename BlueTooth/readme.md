## 1. Introduction
> Bluetooth works in a similar manner to Wi-Fi by converting data into radio frequency waves in the 2.4GHz spectrum. But unlike Wi-Fi, which has a range of 50 metres indoors and twice as much outdoors, Bluetooth is more concerned with keeping transmission power low to conserve battery life and avoid radio interference with neighbouring devices. That’s primarily because it operates on the principle of creating a personal area network (PAN), otherwise also known as the piconet.
![image](https://user-images.githubusercontent.com/48615614/158751690-a9232a10-465d-45eb-89ca-3932b1cec367.png)

## 2. Bluetooth vs W-Fi: What Separates the Two
![image](https://user-images.githubusercontent.com/48615614/158752126-bac25086-86d1-4a7a-a039-81c6b2b61126.png)

## 3. Bluetooth Versions
![image](https://user-images.githubusercontent.com/48615614/158752266-50ea4e10-f207-41c9-9d47-603669f4128e.png)

### There are three basic means of providing Bluetooth security:
* Authentication: In this process the identity of the communicating devices are verified. User authentication is not part of the main Bluetooth security elements of the specification.
* Confidentiality: This process prevents information being eavesdropped by ensuring that only authorised devices can access and view the data.
* Authorisation: This process prevents access by ensuring that a device is authorised to use a service before enabling it to do so.

## 4. The most common types of Bluetooth attacks
### BlueJacking
![image](https://user-images.githubusercontent.com/48615614/158753049-014e4618-ad69-448a-897a-8eb234e6e8e7.png)

This type of cyber attack involves one Bluetooth-enabled device hijacking another and sending spam messages to the hijacked device. Mostly it is an annoyance, but if a recipient falls for such a phishing attempt and clicks on a link in one of these spam messages, bigger issues can arise. The link often takes you to a website where your personal information is stolen or malware is installed on your device.

### BlueSnarfing
![image](https://user-images.githubusercontent.com/48615614/158753362-f627511f-21c1-4965-884b-051b1a2d8c50.png)

A blueSnarfing attack is similar to bluejacking, but more sinister. Where bluejacking only sends information to your device, blueSnarfing also extracts information from your device. Data like text messages, photos, emails, and even the identifying information your device sends to your ISP can all be stolen. The hacker can use this information for a variety of purposes, none of them good.

### BlueBugging
![image](https://user-images.githubusercontent.com/48615614/158753521-62e1900f-7425-4b28-9579-c3e0a59d929a.png)

Here, hackers establish a surreptitious Bluetooth connection with your phone or laptop. They then use this connection to gain backdoor access to your device. Once in, they can spy on your activity, access your sensitive information, and even use your device to impersonate you on any apps on your device, including the apps you use for online banking.
This kind of attack is called bluebugging because it resembles the way one might bug a phone. Once control over the phone is established, cybercriminals can use it to call themselves and listen in on conversations.

### BlueBorn attack (Under research)
### Bluewave Zero-Click Bugs (Under research)
### BlueFrag leak (Under research)

### Bluetooth Low Energy Hacking
Understanding Bluetooth LE communication and demonstrating Man-in-The-Middle attack on Bluetooth LE devices.

### Introduction
Bluetooth Low Energy, also known as BLE, Bluetooth LE, or Bluetooth Smart, is a wireless technology designed to
reduce power consumption and cost. Bluetooth LE can operate on a single coin battery for months to years depending
on usage. Bluetooth LE is more focused on healthcare, fitness, security and home entertainment industries.

### BLE Communication Process
The Bluetooth LE connection and communication process includes the following steps:
* Phase 1: Smart Device broadcasts an advertisement packet. Device also known as Peripheral device
* Phase 2: Smart Phone scans for advertisements packets. Smartphone also known as Central device
* Phase 3: Once specific advertisement packet is received by Central device, it stops scanning further
* Phase 4: Central device initiates the connection to peripheral device
* Phase 5: Central device browses the peripheral for available services
* Phase 6: Central device exchanges information with peripheral device

### Security Specification for BLE
Encryption: Bluetooth LE devices set up the Long-Term Key (LTK) in BLE 4.2 devices, Short Term Key (STK) in BLE 4.0 and 4.1 devices during the pairing process to encrypt transmission to secure further communication.

Encryptions are:
* Just Works
This method uses the static PIN "000000" as Temporary Key (TK) during the pairing process to generate STK for encrypting the transmission.

* Passkey Entry
This method uses a six-digit number as Temporary Key (TK) that is exchanged between pairing devices. In it, user interaction is required to complete the process.

* Out of Band
The Temporary Key is exchanged using different wireless technology, such as NFC for further process into transmission encryption.

* Random MAC Address
Device has change in MAC address on random basis, which only a paired device is able to identify.

* Whitelisting
Whitelisting rule can be implemented for Device's MAC addresses, which are allowed to be accepted.


## Bluetooth LE Attacks
BLE communication is vulnerable to a variety of security issues. The following are the most well-known attacks on smart device communication.

* `Man-in-The-Middle (MiTM)`
An attacker could clone the original device and broadcast advertisement packets more frequently than the original BLE device. The first advertisement packet received by the central device (mobile phone) will be interrupted, it thinks the device is the intended one and starts the connection process. Once the central device starts the connection process, it will stop scanning. Now the victim is tricked and connected to the clone device. This attack is also known as Active Eavesdropping or sniffing. We will see the demonstration of MiTM attack in this article.

* `Replay`
This type of attack includes capturing the request generated by the central device to a peripheral device for some action and plays the same request again and again to perform the same action. For example, an attacker captures the request of turning off the anti-theft tracking device and plays it whenever required. This way, an attacker is able to take advantage of the issue.

* `Denial of Service (DoS)`
In this attack, an attacker clones the original device and starts broadcasting the advertisement packets. But after connecting to the victim, it does not share services or does not set up the services for cloned device, which results in the central device continuing to scan for services for a long finite time before it tries to reconnect.

* `Unauthenticated Access`
An attacker could access the services that are not protected against un-authentication. This may allow access to sensitive data or configuration options that are not allowed to be accessed publicly.

* `Jamming`
Bluetooth LE is prone to jamming attacks like any other wireless technology.

* `Sniffing/Eavesdropping (Passive)`
Just Works and Passkey Entry do not provide passive eavesdropping protection. Unencrypted transmission can be intercepted by a passive eavesdropper such as "Ubertooth One".

https://shop.hak5.org/products/ubertooth-one

https://hackerwarehouse.com/product/ubertooth-one/ 

https://alexnld.com/product/ubertooth-one-2-4-ghz-wireless-development-bluetooth-btle-tool-bluetooth-protocol-analysis-open-source/?gclid=CjwKCAjwlcaRBhBYEiwAK341jSohBaTxUMfRVrei2txFu09_M9jH0Z7unOtMqG6Ru1Db0KDVy8-ouxoC1oAQAvD_BwE

https://ubertooth.readthedocs.io/en/latest/index.html

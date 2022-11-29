### VOIP Penetration testing

Cyber-attacks pose numerous threats to VoIP telephone systems, such as the capture of inbound and outbound calls through network manipulation, recording or listening in on calls, gaining access to internal networks through the voice VLANS and use of the network to make outbound calls (toll fraud).

In order to facilitate correct operation of VoIP devices, VoIP systems often operate outside of normal network security controls. We are able to assist you in securing your VoIP systems.

We will assess your network infrastructure, VoIP components and authentication methods, and their capability to prevent manipulation between your clients and VoIP server to determine the scope of your security, providing a detailed report and recommendations for remediating issues and vulnerabilities.

Our testing generally includes investigating the authentication mechanisms, as well as the potential interception, interruption or manipulation of the exchanged information between the client and VoIP server.

What are we looking to do when conducting a VoIP Penetration Test? The goals are not dissimilar to that of a traditional network penetration test:

	• Information Gathering, Footprinting and Enumeration.
	• Monitoring Traffic and eavesdropping Phone calls.
	• Attacking Authentication.
	• VLAN Hopping.
	• Denial of Service / Flooding.
	• Spoofing Caller ID.

In order to achieve these goals, a VoIP-focused methodology is extremely helpful so that these types of assessments can be both repeatable and consistent over the course of time. The methodology is similar to that of a traditional penetration test:

#### Phase 1: Gather Information

What types of phones are in use? Who makes them? Model? Where is the SIP Server? What’s the software in use? Are they using TCP or UDP for traffic? Are they sending it through secure means? What are their IPs and what other ports are open on these devices? These are all questions that need to be answered during the initial phase of the penetration test.

#### Phase 2: Enumeration

concentrate on finding extensions and usernames/passwords for users across the VoIP network. The main focus of this phase is to find your way(s) in. Utilizing who and how someone uses the VoIP is key when trying to get in.

#### Phase 3: Exploit

This is the phase where the magic happens. In this phase, the aforementioned goals are tested. One tries to grab whatever information they can from calls in progress, send requests, and test the entire VoIP network through traditional means by logging into web management portals, network segmentation, VLAN hopping and so on.


<br><br>

### Introduction

#### VoIP Defined
The term VoIP stands for Voice Over Internet Protocol.  It is a broad term that covers any phone calls made over the Internet, as opposed to traditional telephone lines, otherwise known as the PSTN (Public Switched Telephone Network). Other terms that are used interchangeably with VoIP include, IP telephony, Internet telephony, voice over broadband, broadband telephony, IP communications, and broadband phone service. They all describe the fact that the Internet is used to digitally transmit the voice signal to another telephone or endpoint.



#### Understanding SIP
Session Initiation Protocol (SIP) is a communications protocol that is widely used for managing multimedia communication sessions such as voice and video calls.  SIP, therefore is one of the specific protocols that enable VoIP.   It defines the messages that are sent between endpoints and it governs establishment, termination and other essential elements of a call. SIP can be used to transmit information between just two endpoints or many.  In addition to voice, SIP can be used for video conferencing, instant messaging, media distribution and other applications.



#### Understand Call Requirements

It’s essential to understand VoIP Call Requirements before starting the testing You don’t want to run security tests without understanding how VoIP fits into their business.

 Some pre-scoping questions:

• What VoIP applications are being used on the network?
• What are the VoIP call scenarios for users?
• Are soft phones being used? What part of the network are users able to use their soft phones?
• What are the HA requirements for VoIP calls?
• What VoIP attacks are they most concerned in protecting against?




#### Understand VLAN & QoS Design

Why?

	• We only want to be testing against VoIP infrastructure. We don’t want to test against Data network.
	• It’s important to understand the network design because VoIP is so tightly integrated into the network.

	1. QoS is essential to VoIP network infrastructure design and implementation.
	2. Is VoIP running on a flat network, or segmented with distinct VLANS for QoS?
	3. We can use technical methods to determine this on our own, blindly.
	4. The “Voice VLAN” is a special access port feature that allows IP Phones to easily configure themselves into a separate VLAN, for QoS.
	5. Most current VoIP deployments use the Voice VLAN feature.
	6. It’s considered a best practice of VoIP to implement QoS along with Voice VLANs, so we will only see the prevalence increasing. 


#### The Voice VLAN Feature 

	1. Allows IP Phones to easily auto-configure
	2. Provides easy QoS
	3. Phones easily associate to a logically separate VLAN
	4. Saves on cabling costs
	5. Allows simultaneous access for a PC





















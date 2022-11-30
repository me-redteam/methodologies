### VLAN Hopping

Usually VoIP traffic is connected to a dedicated VLAN (Virtual LAN) as we saw in the topologies section. This means that we cannot intercept the VoIP traffic by sniffing and Arp poisoning. The reason for that is that a VLAN is like a separate network, it has its own broadcast domain and different IP range than the data network. VLAN hopping is a way to “hop” to another VLAN. One common topology is where the IP Phone has a built-in “Internal Switch”, usually the pc is plugged into the phone pc socket and the phone is connected from its lan/sw socket to the network switch as follows: 

![image](https://user-images.githubusercontent.com/48615614/204712262-c527445e-2e54-491e-a2bb-91eaa2d254f2.png)

#### VoIP Hopper
VoIP hopper is used to hop into voice Vlan by behaving like an IP phone, it supports specific switches and supports some IP phones models. It currently supports the brands like: Cisco, Avaya and Nortel. VoIP hopper was designed to run under Backtrack Linux and currently has the following features: `DHCP Client`, `CDP Generator`, `MAC Address Spoofing` and `VLAN hopping`. Voiphopper usage: 

![image](https://user-images.githubusercontent.com/48615614/204712317-0dcebe2b-b787-4253-8b36-1abc89196379.png)

VoIP Hopper provides many modes for attack please use the `–h` for detailed information.

Let’s take a look at an example of sniffing for CDP and run a VLAN Hop into the Voice VLAN in a Cisco environment. Run VoIP Hopper on the Ethernet interface, in the following way: 

![image](https://user-images.githubusercontent.com/48615614/204712511-69c9f322-7e74-4667-be02-d38dae3f218b.png)

![image](https://user-images.githubusercontent.com/48615614/204712540-c2f2761c-050a-4d86-9fa6-dca2134a900e.png)

VoIP Hopper also allows one to VLAN Hop to an arbitrary VLAN, without sniffing for CDP. If you already know the Voice VLAN ID or would like to VLAN Hop into another VLAN just specify the vlan id. 

![image](https://user-images.githubusercontent.com/48615614/204712619-c14560db-92ce-4b88-8d24-a0e70be5a48b.png)

#### ACE
`ACE` is another tool for vlan hopping very similar to `Voiphopper` in usage and include an option to discover also TFTP servers (configuration servers). ACE Usage:

![image](https://user-images.githubusercontent.com/48615614/204712661-bfac6db0-2bf1-40c8-9f19-a43f780032dc.png)

You can manually add a vlan hop or use its discovery feature 

![image](https://user-images.githubusercontent.com/48615614/204712804-ff0c46cd-a227-45cc-8a50-b4c18a778c84.png)

TIP: To view your MAC address in backtrack use: 

![image](https://user-images.githubusercontent.com/48615614/204712842-77c33f78-322a-4793-9bcc-379eba69b520.png)

![image](https://user-images.githubusercontent.com/48615614/204712993-a16eb507-858c-49ca-82a4-c407310f0b8f.png)

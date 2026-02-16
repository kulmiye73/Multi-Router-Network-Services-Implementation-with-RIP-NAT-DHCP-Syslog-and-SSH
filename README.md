# Multi-Router-Network-Services-Implementation-with-RIP-NAT-DHCP-Syslog-and-SSH
## Designed a multi-router Cisco network using RIP v2 for dynamic routing. Implemented PAT for internet access, centralized DHCP with relay, Syslog monitoring, and SSH v2 secure management, demonstrating core CCNA enterprise networking skills.

 

Multi-Router Network Services Implementation with RIP, NAT, DHCP, Syslog, and SSH

Lab details.

1. Configure RIP between R1, R2, and R3, advertising all connected networks.
-Use RIP version 2
-Disable auto-summary

2. Configure R1, R2, and R3 to send Syslog messages to SRV1

3. Configure PAT on R1 and R2 to translate their inside hosts to their G0/1 interface

### 4. Configure R1 as a DHCP server with two pools:
#### 1pool:

Network: 192.168.1.0/24

Default gateway: 192.168.1.1

DNS server: 30.0.0.100

Excluded range: 192.168.1.1 - 192.168.1.10

#### 2pool:
Network: 192.168.2.0/24

Default gateway: 192.168.2.1

DNS server: 30.0.0.100

Excluded range: 192.168.2.1 - 192.168.2.10


### 5. Configure R2 to forward DHCP requests to R1

### 6. Configure R1 for SSH version 2 access on the VTY lines:

Username: cisco, password: ccna


Domain name: cisco.com


Key modulus: 1024 bit


# Task one

## 1. Configure RIP between R1, R2, and R3, advertising all connected networks.

## -Use RIP version 2

## -Disable auto-summary



R1(config)#router rip

R1(config-router)#version 2

R1(config-router)#network 1.2.3.0

R1(config-router)#network 192.168.1.0

R1(config-router)#no au

R1(config-router)#no auto-summary 

R1(config-router)#

R2(config)#router rip

R2(config-router)#version 2

R2(config-router)#network 1.2.3.0

R2(config-router)#network 192.168.2.0

R2(config-router)#no au

R2(config-router)#no auto-summary 

R2(config-router)#


R3(config)#router rip

R3(config-router)#version 2

R3(config-router)#network 30.0.0.0

R3(config-router)#network 1.2.3.0

R3(config-router)#no auto summary

R3(config-router)#end

R3#

## Use  Show IP  Protocols to check if RIP is configured

R3#show ip protocols 

Routing Protocol is "rip"

Sending updates every 30 seconds, next due in 21 seconds

Invalid after 180 seconds, hold down 180, flushed after 240

Outgoing update filter list for all interfaces is not set

Incoming update filter list for all interfaces is not set

Redistributing: rip

Default version control: send version 2, receive 2

Interface Send Recv Triggered RIP Key-chain

GigabitEthernet0/0 22

GigabitEthernet0/1 22

Automatic network summarization is not in effect

Maximum path: 4

Routing for Networks:

1.0.0.0

30.0.0.0

Passive Interface(s):

Routing Information Sources:

Gateway Distance Last Update

1.2.3.1 120 00:00:23

1.2.3.2 120 00:00:12

Distance: (default is 120)

## Check if three routers can communicate using ping

R1#ping 1.2.3.3

Type escape sequence to abort.

Sending 5, 100-byte ICMP Echos to 1.2.3.3, timeout is 2 seconds:

!!!!!

Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms


R1#ping 192.168.2.1


Type escape sequence to abort.

Sending 5, 100-byte ICMP Echos to 192.168.2.1, timeout is 2 seconds:

!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/1 ms




# Task Two

## Configure R1, R2, and R3 to send Syslog messages to SRV1

R1(config)#logg

R1(config)#logging host 30.0.0.100

R1(config)#logg

R1(config)#logging tr

R1(config)#logging trap 

R1(config)#logging trap information


R2#conf t

Enter configuration commands, one per line. End with CNTL/Z.

R2(config)#logg

R2(config)#logging host 30.0.0.100

R2(config)#logging tr

R2(config)#logging trap 

R2(config)#logging trap information 

R2(config)#logging on


R3#conf t

Enter configuration commands, one per line. End with CNTL/Z.

R3(config)#logging host 30.0.0.100

R3(config)#logging trap debugging

R3(config)#











## First check the IP  of the  server and enable syslog server

 
# Task three.

## 3. Configure PAT on R1 and R2 to translate their inside hosts to their G0/1 interface

R1>

R1>en

R1#conf t

Enter configuration commands, one per line. End with CNTL/Z.

R1(config)#int g0/0

R1(config-if)#ip nat inside

R1(config-if)#exit

R1(config)#int g0/1

R1(config-if)#ip nat outside

R1(config-if)#exit

R1(config)#acc

R1(config)#access-list 1 permit 192.168.1.0 0.0.0.255

R1(config)#ip nat inside source

R1(config)#ip nat inside source l

R1(config)#ip nat inside source list 1 ?

interface Specify interface for global address

pool Name pool of global addresses

R1(config)#ip nat inside source list 1 int g0/1 o

R1(config)#ip nat inside source list 1 int g0/1 overload 

R1(config)#


R2>

R2>en

R2#conf t

Enter configuration commands, one per line. End with CNTL/Z.

R2(config)#int g0/0

R2(config-if)#ip nat inside

R2(config-if)#exit

R2(config)#int g0/1

R2(config-if)#ip nat outside

R2(config-if)#exit

R2(config)#access

R2(config)#access-list 1 permit 192.168.2.0 0.0.0.255

R2(config)#ip nat inside source lis 1 int g0/1 o

R2(config)#ip nat inside source lis 1 int g0/1 overload 

R2(config)#


# Task four

## 4. Configure R1 as a DHCP server with two pools:

## 1pool:

Network: 192.168.1.0/24

Default gateway: 192.168.1.1

DNS server: 30.0.0.100

Excluded range: 192.168.1.1 - 192.168.1.10


## 2pool:

Network: 192.168.2.0/24

Default gateway: 192.168.2.1

DNS server: 30.0.0.100

Excluded range: 192.168.2.1 - 192.168.2.10


R1(config)#ip dhcp pool 1pool

R1(dhcp-config)#network 192.168.1.0 255.255.255.0

R1(dhcp-config)#dns

R1(dhcp-config)#dns-server 30.0.0.100

R1(dhcp-config)#de

R1(dhcp-config)#default-router 192.168.1.1

R1(dhcp-config)#ip dhcp ex

R1(dhcp-config)#exit

R1(config)#ip dhcp e

R1(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.10

R1(config)#ip dhcp pool 2pool

R1(dhcp-config)#network 192.168.2.0 255.255.255.0

R1(dhcp-config)#dns

R1(dhcp-config)#dns-server 192.168.2.1

R1(dhcp-config)#de

R1(dhcp-config)#default-router 192.168.2.1

R1(dhcp-config)#no dns-server 192.168.2.1

R1(dhcp-config)#dns

R1(dhcp-config)#dns-server 30.0.0.100

R1(dhcp-config)#exit

R1(config)#ip dhcp ex

R1(config)#ip dhcp excluded-address 192.168.2.1 192.168.2.10

R1(config)#

# Task Five

## 5. Configure R2 to forward DHCP requests to R1

R2(config)#int g0/0

R2(config-if)#ip helper-address 1.2.3.1

R2(config-if)#

## Now we can check if PC4 get ip address from dhcp 2pool

C:\>ipconfig /renew

IP Address......................: 192.168.2.13

Subnet Mask.....................: 255.255.255.0

Default Gateway.................: 192.168.2.1

DNS Server......................: 30.0.0.100


# Task Six

## 6. Configure R1 for SSH version 2 access on the VTY lines:
Username: cisco, password: ccna
Domain name: cisco.com
Key modulus: 1024 bit


R1#conf t

Enter configuration commands, one per line. End with CNTL/Z.

R1(config)#ip domain-name cisco.com

R1(config)#username cisco password ccna

R1(config)#cr

R1(config)#crypto k

R1(config)#crypto key g

R1(config)#crypto key generate rs

R1(config)#crypto key generate rsa

The name for the keys will be: R1.cisco.com

Choose the size of the key modulus in the range of 360 to 4096 for your

General Purpose Keys. Choosing a key modulus greater than 512 may take

a few minutes.

How many bits in the modulus [512]: 1024

% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]

R1(config)#

*Feb 28 9:6:3.417: %SSH-5-ENABLED: SSH 2 has been enabled

R1(config)#ip ssh version 2

R1(config)#line vty 0

R1(config-line)#login local

R1(config-line)#tr

R1(config-line)#transport i

R1(config-line)#transport input ssh

R1(config-line)#

R1#

R1#show ip ssh

SSH Enabled - version 2.0

Authentication timeout: 120 secs; Authentication retries: 3

### Now we can remotely access from PC3 to Router1 using this command 

C:\>

C:\>ssh -l cisco 192.168.1.1


Password: 




R1>en

Password: 

R1#conf t

Enter configuration commands, one per line. End with CNTL/Z.

R1(config)#



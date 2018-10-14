# Channel-Bonding-interfaces-LACP---Switch-Centos7

![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/images/ether1.png)

# Router Configuration 

![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/images/ether5.png)

# Switch Configuration 

![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/images/ether4.png)

### Switch side configuration 


~~~
SW#sh ip int br
~~~

~~~
Interface              IP-Address      OK? Method Status                Protocol
Ethernet0/0            unassigned      YES unset  up                    up      
Ethernet0/1            unassigned      YES unset  up                    up      
Ethernet0/2            unassigned      YES unset  up                    up      
Ethernet0/3            unassigned      YES unset  up                    up      
Ethernet1/0            unassigned      YES unset  up                    up      
Ethernet1/1            unassigned      YES unset  up                    up      
Ethernet1/2            unassigned      YES unset  up                    up      
Ethernet1/3            unassigned      YES unset  up                    up      
Vlan1                  unassigned      YES unset  administratively down down    
SW#
~~~

### Vlan Database 

~~~
conf t
vlan 10  
name ExternalNetworkVlanID
vlan 20  
name StorageNetworkVlanID
vlan 30  
name InternalApiNetworkVlanID
vlan 40  
name StorageMgmtNetworkVlanID
vlan 50  
name TenantNetworkVlanID
vlan 100 
name provisioning
end
wr
~~~

### Check vlan database details 


~~~
SW# sh vlan br
~~~

~~~
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Et0/0, Et0/1, Et0/2, Et0/3
						Et1/0, Et1/1, Et1/2, Et1/3	
10   ExternalNetworkVlanID            active    
20   StorageNetworkVlanID             active    
30   InternalApiNetworkVlanID         active    
40   StorageMgmtNetworkVlanID         active    
50   TenantNetworkVlanID              active    
100  provisioning                     active    
1002 fddi-default                     act/unsup 
1003 token-ring-default               act/unsup 
1004 fddinet-default                  act/unsup 
1005 trnet-default                    act/unsup 
SW#
~~~


### VTP CONFIGURATION ### 

~~~
conf t
vtp mode server 
vtp domain OPENSTACKLAB
exit
wr
~~~

### TRUNK PORT CONFIGURATION FOR VLAN RANGE 10-50 ###


~~~
configure terminal 
interface range Ethernet 1/0 - 3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 1-4,10-50,1002-1005
end
wr
~~~

### STP CONFIGURATION
~~~
conf t
interface range Ethernet 1/0 - 3
switchport mode trunk
spanning-tree portfast
exit
exit
wr
~~~

### Check Trunk Details 

~~~
ESW1# sh interfaces trunk 
~~~

~~~
Port      Mode         Encapsulation  Status        Native vlan
Eth1/0    on           802.1q         trunking      1
Eth1/1    on           802.1q         trunking      1
Eth1/2    on           802.1q         trunking      1
Eth1/3    on           802.1q         trunking      1

Port      Vlans allowed on trunk
Eth1/0    1-4,20-50,1002-1005
Eth1/1    1-4,20-50,1002-1005
Eth1/2    1-4,20-50,1002-1005
Eth1/3    1-4,20-50,1002-1005

Port      Vlans allowed and active in management domain
Eth1/0    1-4,20-50,1002-1005
Eth1/1    1-4,20-50,1002-1005
Eth1/2    1-4,20-50,1002-1005
Eth1/3    1-4,20-50,1002-1005

Port      Vlans in spanning tree forwarding state and not pruned
Eth1/0    1-4,20-50,1002-1005
Eth1/1    1-4,20-50,1002-1005
Eth1/2    1-4,20-50,1002-1005
Eth1/3    1-4,20-50,1002-1005
ESW1#
~~~

# Switch EtherChannel Configuration 

### Channel Group 1 for port 1/0 - 1

~~~
conf t
interface range Ethernet 1/0 - 1
channel-group 1 mode active
end
wr
~~~

### Channel Group 2 for port 1/2 - 3

~~~
conf t
interface range Ethernet 1/2 - 3
channel-group 2 mode active
end
wr
~~~

### EtherChannel Summary (LACP) 

![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/images/ether6.png)

### Interface ``TRUNK`` Details

![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/images/ether7.png)

# Server Side Configuration (Centos-7) 

### Both the Centos-7 nodes having 3 interfaces each, we are using only ``ens4`` & ``ens5`` on both the server. 

![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/images/ether8.png)

### bond0 

~~~
[root@localhost ~]# cat /proc/net/bonding/bond0 
~~~

~~~
Ethernet Channel Bonding Driver: v3.7.1 (April 27, 2011)

Bonding Mode: IEEE 802.3ad Dynamic link aggregation
Transmit Hash Policy: layer2 (0)
MII Status: up
MII Polling Interval (ms): 100
Up Delay (ms): 0
Down Delay (ms): 0

802.3ad info
LACP rate: fast
Min links: 0
Aggregator selection policy (ad_select): stable
System priority: 65535
System MAC address: 0c:06:72:c7:e1:01
Active Aggregator Info:
	Aggregator ID: 1
	Number of ports: 2
	Actor Key: 9
	Partner Key: 1
	Partner Mac Address: aa:bb:cc:80:01:00

Slave Interface: ens4  <==================== for interface ens4
MII Status: up
Speed: 1000 Mbps
Duplex: full
Link Failure Count: 0
Permanent HW addr: 0c:06:72:c7:e1:01
Slave queue ID: 0
Aggregator ID: 1
Actor Churn State: none
Partner Churn State: none
Actor Churned Count: 1
Partner Churned Count: 1
details actor lacp pdu:
    system priority: 65535
    system mac address: 0c:06:72:c7:e1:01
    port key: 9
    port priority: 255
    port number: 1
    port state: 63
details partner lacp pdu:
    system priority: 32768
    system mac address: aa:bb:cc:80:01:00
    oper key: 1
    port priority: 32768
    port number: 257
    port state: 61

Slave Interface: ens5  <==================== for interface ens5 
MII Status: up
Speed: 1000 Mbps
Duplex: full
Link Failure Count: 0
Permanent HW addr: 0c:06:72:c7:e1:02
Slave queue ID: 0
Aggregator ID: 1
Actor Churn State: none
Partner Churn State: none
Actor Churned Count: 1
Partner Churned Count: 2
details actor lacp pdu:
    system priority: 65535
    system mac address: 0c:06:72:c7:e1:01
    port key: 9
    port priority: 255
    port number: 2
    port state: 63
details partner lacp pdu:
    system priority: 32768
    system mac address: aa:bb:cc:80:01:00
    oper key: 1
    port priority: 32768
    port number: 258
    port state: 61
[root@localhost ~]# 
~~~

### Server ``A`` - Centos IP's 


![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/images/ether9.png)


### Server ``B`` - Centos IP's 

 
![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/images/ether10.png)

### Ping test between Server A and B 

![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/images/ether11.png)


# LACP Testing between Switch and Both Server. 

### Test 1 - Ping from A (10.10.10.100) to (10.10.10.200) , On switch will down port Eth1/2 and Eth1/3


![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/images/ether12.png)

### Down port 1/2 , Still no ``PING`` loss

![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/images/ether13.png)


### Down port 1/3 , ``PING`` loss, as both port is ``DOWN``


![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/images/ether15.png)



### ``UP`` port 1/3 , no ``PING`` loss, NOTE : only one port  is ``UP``

![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/images/ether16.png)


### ``UP`` port 1/2 , no ``PING`` loss, NOTE : ``BOTH``  port  is ``UP``

![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/images/ether17.png)

# Channel-Bonding-interfaces-LACP---Switch-Centos7

![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/ether1.png)

# Router Configuration 

![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/ether5.png)

# Switch Configuration 

![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/ether4.png)

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

### Channel Group 1 for port 1/2 - 3

~~~
conf t
interface range Ethernet 1/2 - 3
channel-group 2 mode active
end
wr
~~~

### EtherChannel Summary (LACP) 

![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/ether6.png)

### Interface ``TRUNK`` Details

![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/ether7.png)

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


![Image vlan](https://github.com/NileshChandekar/Channel-Bonding-interfaces-LACP---Switch-Centos7/blob/master/ether4.png)

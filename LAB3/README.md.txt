DAY3 VLAN CONFIGURATION STEP BY STEP USING CISCO PACKET TRACER
2 SWITCHES (2960-24TT)

SWITCH 1 CONNECTED PCs
2PC VLAN 10 192.168.10.0/24
2PC VLAN 20 192.168.20.0/24
2PC VLAN 30 192.168.30.0/24

SWITCH 2 CONNECTED PC's
2PC VLAN 10 192.168.10.0/24
2PC VLAN 20 192.168.20.0/24
2PC VLAN 30 192.168.30.0/24

We are going to perform:::---
Draw network topology and label VLANs.
Assign IP address to each PC based on VLAN.
Create VLANs on the switch.
Assign switch ports to correct VLANs.
Configure trunk link between switches.
Test communication using ping.

✔ Same VLAN → Communication works
✖ Different VLAN → Communication fails by default


assign ip address to each host
on switch 1 go to configuration terminal
vlan 10
name IT
exit
vlan 20 
name HR
exit
vlan 30
name FIN 
exit

same step on switch 2 go to configuration terminal
vlan 10
name IT
exit
vlan 20 
name HR
exit
vlan 30
name FIN 
exit

do show vlan 

assign vlan to ports
in switch1

interface fastEthernet 0/1
switchport mode access
switchport access VLAN 10
exit

interface fastEthernet 0/2
switchport mode access
switchport access VLAN 10
exit

interface fastEthernet 0/3-4
switchport mode access
switchport access VLAN 20
exit

interface fastEthernet 0/5-6
switchport mode access
switchport access VLAN 30
exit


in switch2

interface fastEthernet 0/1-2
switchport mode access
switchport access VLAN 10
exit

interface fastEthernet 0/3-4
switchport mode access
switchport access VLAN 20
exit

interface fastEthernet 0/5-6
switchport mode access
switchport access VLAN 30
exit


trunk both switches
in switch 1
interface fastEthernet 0/7
switchport mode trunk

in switch 2
interface fastEthernet 0/7
switchport mode trunk

now test 
go to pc one
and in command promt 
ping 192.168.10.30
check it  success or fail
then ping different vlan pc
ping 192.168.20.30
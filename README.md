# Simple-VLAN-Configuration
A VLAN configuration and isolation of multiple devices on a single network divided by two Switches

We have four end devices and two switches on the same local network, 10.0.0./24.
we can isolate devices at layer 2 using VLAN.
![1](https://github.com/kelubia/Simple-VLAN-Configuration/assets/98921903/278c5bf0-5f85-4198-a31d-19bd95a270c2)
to properly isolate the VLANs 
STEP1
ping all devices to be sure of proper connectivity
![2](https://github.com/kelubia/Simple-VLAN-Configuration/assets/98921903/c2c9fe28-d165-47f8-8cc0-468da34222b9)

All our PCs are connected. 

step 2
assign pc1 and pc2 to VLAN 1
and PC 3 and PC4 to VLAN 2
this is done by configuring the switches they are connected to, starting with switch one
PC1 - f0/2
switch ports connected to the end host are configured as access ports
we will configure the regular SW1 switch port into VLan 1
![3](https://github.com/kelubia/Simple-VLAN-Configuration/assets/98921903/2e91a2c4-dfc4-4ebd-b550-d32d3f822829)

next, configure pc 2-f0/3, adding it to the VLAN
check the running config to show configurations
![4](https://github.com/kelubia/Simple-VLAN-Configuration/assets/98921903/8d5afc30-0840-4607-9b18-cc780758a3f7)

next, config SW2 switch
######code#######
 SW2>enable
SW2#conf t
Enter configuration commands, one per line. End with CNTL/Z.
SW2(config)#int f0/2
SW2(config-if)#sw
SW2(config-if)#switchport mo
SW2(config-if)#switchport mode ac
SW2(config-if)#switchport mode access 
SW2(config-if)#swi
SW2(config-if)#switchport acc
SW2(config-if)#switchport access vlan 1
SW2(config-if)#int f0/3
SW2(config-if)#swi
SW2(config-if)#switchport mode ac
SW2(config-if)#switchport mode access 
SW2(config-if)#swit
SW2(config-if)#switchport acc
SW2(config-if)#switchport access vlan 2
% Access VLAN does not exist. Creating VLAN 2
SW2(config-if)#
![5](https://github.com/kelubia/Simple-VLAN-Configuration/assets/98921903/d8307ff3-ca02-454e-a669-d1cb3c68802d)

show the running config
###SW2(config-if)#do sh run
![6](https://github.com/kelubia/Simple-VLAN-Configuration/assets/98921903/23080d7e-9939-44a1-8273-886fc5d20119)

attempt pinging by pc1 and three and pc 2 and 4 in the same VLAN
PC1 ping PC3
![7](https://github.com/kelubia/Simple-VLAN-Configuration/assets/98921903/fd92f572-a2a3-4407-9dc3-42eafad1063e)

PC2 ping PC4 failed cause, by default, only ping from the native vlan 1 PC1 can go through
![8](https://github.com/kelubia/Simple-VLAN-Configuration/assets/98921903/8c43787b-9df2-4945-8913-b15947796ddc)

Configure SW1 and SW2 as trunk interfaces to allow traffic from multiple VLANs to pass between switches.
in SW1
#####
SW1>enable
SW1#conf t
Enter configuration commands, one per line. End with CNTL/Z.
SW1(config)#int f0/1
SW1(config-if)#switch mode trunk
SW1(config-if)#
![9](https://github.com/kelubia/Simple-VLAN-Configuration/assets/98921903/9f35ff4f-18df-4f10-ab2a-33b091e57585)

Do the same for SW2
######
SW2>enable
SW2#conf t
Enter configuration commands, one per line. End with CNTL/Z.
SW2(config)#int f0/1
SW2(config-if)#switch
SW2(config-if)#switchport mode trunk
![10](https://github.com/kelubia/Simple-VLAN-Configuration/assets/98921903/5f46e6c0-50e8-4639-a822-2a93680d2d8b)

If our configurations are appropriate, we can ping between PC1-PC3 and PC2-PC4.
PC1-PC2 and PC4 will time out, while PC1-PC3 will succeed
![11](https://github.com/kelubia/Simple-VLAN-Configuration/assets/98921903/3ef992c0-9203-4002-a997-76127e3dbd00)

PC2-PC1 and PC3 will fail while PC2-PC4 will succeed
![12](https://github.com/kelubia/Simple-VLAN-Configuration/assets/98921903/6cbb8252-4ec1-409e-9dca-cd97d16b2027)


We have isolated our networks between 4 VLANs despite sharing the same network.




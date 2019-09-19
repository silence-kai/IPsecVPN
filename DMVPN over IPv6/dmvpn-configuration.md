![Image](https://github.com/silence-kai/IPsecVPN/blob/master/DMVPN%20over%20IPv6/dmvpnV6.png)

Example of CISCO ISR4221:  
  
- HUB1:  
interface gi0/0  
　　ipv6 address 2002::1 64  
crypto keyring dmvpn   
　　pre-shared-key address ipv6 :: key ***  
crypto isakmp profile dmvpn  
　　keyring dmvpn   
　　match identity address ipv6 ::  
crypto ipsec transform-set dmvpn esp-3des esp-sha-hmac    
　　mode transport  
crypto ipsec profile dmvpn  
　　set transform-set dmvpn  
　　set isakmp-profile dmvpn  
interface Tunnel1  
　　ipv6 address FC00:1:: 64  
　　ipv6 address FE80::1 link-local   
　　ipv6 mtu 1420  
　　ipv6 nhrp authentication cntdmvpn  
　　ipv6 nhrp map multicast dynamic  
　　ipv6 nhrp map multicast 2003::1  
　　ipv6 nhrp map FE80::2 2003::1  
　　ipv6 nhrp network-id 1  
　　ipv6 nhrp nhs FE80::2  
　　ospfv3 network broadcast  
　　ospfv3 priority 255  
　　ospfv3 1 ipv6 area 0  
　　tunnel source GigabitEthernet0 /0  
　　tunnel mode gre multipoint ipv6  
　　tunnel protection ipsec profile dmvpn  
ipv6 router ospf 1  
　　router-id 0.0.0.1  
  
  
- HUB2  
interface gi0/0  
    ipv6 address 2003::1 64  
crypto keyring dmvpn  
    pre-shared-key address ipv6 :: key ***  
crypto isakmp profile dmvpn  
    keyring dmvpn  
    match identity address ipv6 ::  　　
crypto ipsec transform-set dmvpn esp-3des esp-sha-hmac   　　
    mode transport  　　
crypto ipsec profile dmvpn  　　
    set transform-set dmvpn  
    set isakmp-profile dmvpn  
interface Tunnel1  
    ipv6 address FC00:2:: 64  
    ipv6 address FE80::2 link-local   
    ipv6 mtu 1420  
    ipv6 nhrp authentication cntdmvpn   
    ipv6 nhrp map multicast dynamic  
    ipv6 nhrp map multicast 2002::1  
    ipv6 nhrp map FE80::1 2002::1  
    ipv6 nhrp network-id 1  
    ipv6 nhrp nhs FE80::1  
　　ospfv3 network broadcast    
　　ospfv3 priority 255    
　　ospfv3 1 ipv6 area 0    
    tunnel source GigabitEthernet0 /0  
    tunnel mode gre multipoint ipv6  
    tunnel protection ipsec profile dmvpn  
ipv6 router ospf 1  
    router-id 0.0.0.2  
  

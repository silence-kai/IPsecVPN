# Here is an issue about the configuration in Cisco ISR4221:  

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
　　ipv6 nhrp nhs FE80::1   
　　ospfv3 network broadcast  #  ipv6 ospf network broadcast  
　　ospfv3 priority 254  #  ipv6 ospf priority 254  
　　ospfv3 1 ipv6 area 1  #  ipv6 ospf network broadcast  

Is "ipv6 ospf network broadcast "equal to "ospfv3 network broadcast" ?  
Is "ospfv3 1 ipv6 area 1" equal "ipv6 ospf network broadcast" ?
because I 've configured it for my company, I have never do the test for this.  

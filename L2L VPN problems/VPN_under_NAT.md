![Image](https://github.com/silence-kai/IPsecVPN/blob/master/L2L%20VPN%20problems/L2L%20VPN%20under%20the%20NAT.png)  


Yep, there are many different ISP in different country, some ISP provide a fiber in your office, but some ISP provide a router in your office.  
If you bought a public ip from ISP, some ISP will install a router in your office, and they will provide a public IP and a private IP, you must configure the private IP on the outbound interface on your edge router, private IP will be NAT in ISP router.  
This is how you access internet, but how could configure the site to site VPN, and make it work properly.  

In general, we will do this in router of site A:  

**crypto kering LANB**  
　　**pre-shared-key address 5.5.5.5 key pasword**  
**crypto isakmp profile LANB**  
　　**vrf LANB**  
　　**keyring LANB**  
　　**match identity address　5.5.5.5 255.255.255.255**  
　　**...**  
　　**...**  
  
OK, you will find the VPN tunnel will not work. so you should change the configuration:  

**crypto kering LANB**  
　　**pre-shared-key address 5.5.5.5 key pasword**  
**crypto isakmp profile LANB**  
　　**vrf LANB**  
　　**keyring LANB**  
　　**match identity address　10.1.1.1 255.255.255.255**  
　　**...**  
　　**...** 
The VPN tunnel is working now.

#LAN to LAN vpn always break down automatically  
I met a strange thing about LAN to LAN VPN, I builld the VPN between site A and site B, but you do command "**show isakmp sa**", you find the tunnel is not up, if the client of A access the client of B, the tunnel still down, otherwise, if the client of B access the client of A, the tunnel will up, here is the configuration:  

Site A(Cisco ASA 5505):  
access-list siteA extended permit ip 192.168.155.0 255.255.255.0 10.100.2.0 255.255.255.0  
access-list acl_nat0 extended permit ip 192.168.155.0 255.255.255.0 10.100.2.0 255.255.255.0  
nat (inside) 0 access-list acl_nat0   
crypto ipsec transform-set siteA esp-3des esp-sha-hmac  
crypto ipsec security-association lifetime seconds 43200  
crypto ipsec security-association lifetime kilobytes 4608000  
crypto map mymap 10 match address siteA  
crypto map mymap 10 set peer a.a.a.a  
crypto map mymap 10 set transform-set siteA  
crypto map mymap interface outside  
crypto isakmp identity address  
crypto isakmp enable outside  
crypto isakmp policy 20  
authentication pre-share  
encryption 3des  
hash sha  
group 2  
lifetime 86400  
tunnel-group a.a.a.a type ipsec-l2l  
tunnel-group a.a.a.a ipsec-attributes  
pre-shared-key 123456  

Site B(Cisco ASA 5505):  
access-list nat0 extended permit ip 10.100.2.0 255.255.255.0 192.168.155.0 255.255.255.0  
access-list siteB extended permit ip 10.100.2.0 255.255.255.0 192.168.155.0 255.255.255.0  
nat (inside) 0 access-list nat0  
crypto ipsec transform-set siteB esp-3des esp-sha-hmac  
crypto ipsec security-association lifetime seconds 43200  
crypto ipsec security-association lifetime kilobytes 4608000  
crypto map mymap 10 match address siteB  
crypto map mymap 10 set peer b.b.b.b  
crypto map mymap 10 set transform-set siteB  
crypto map mymap interface outside  
crypto isakmp identity address  
crypto isakmp enable outside  
crypto isakmp policy 20  
authentication pre-share  
encryption 3des  
hash sha       
group 2  
lifetime 86400  
tunnel-group b.b.b.b type ipsec-l2l  
tunnel-group b.b.b.b ipsec-attributes  
pre-shared-key 123456  

it looks so good, but someone could see that the tunnel is down unless traffic through the tunnel, if there is no traffic throw the tunnel for few minutes, the tunnel will be down again. it can only be accessed in one direction, not in both directions.  
so please check if there is a easy VPN on your firewall:  
  
**crypto dynamic-map dyn 2 set transform-set easyvpn**  
**crypto dynamic-map dyn 2 set reverse-route**  
**crypto map mymap 2 ipsec-isakmp dynamic dyn**  

yep, you could see the number of "crypto dynamic-map" is less than "crypto map", so you should change it more than the number of "crypto map":  

**crypto dynamic-map dyn 100 set transform-set easyvpn**  
**crypto dynamic-map dyn 100 set reverse-route**  
**crypto map mymap 100 ipsec-isakmp dynamic dyn**  
  
well, the tunnel is working properly. you could do "**show isakmp sa**", the tunnel never be down, always in up state.

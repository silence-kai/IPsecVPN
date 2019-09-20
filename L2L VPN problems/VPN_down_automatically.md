#LAN to LAN vpn always break down automatically  
I met a strange thing about LAN to LAN VPN, I builld the VPN between site A and site B, but you do the "**show isakmp sa**", you find the tunnel is not up, if the client of A access the client of B, the tunnel still down, otherwise, if the client of B access the client of A, the tunnel will up, here is the configuration:  

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
crypto map cienetvpn 150 match address siteB  
crypto map cienetvpn 150 set peer b.b.b.b  
crypto map cienetvpn 150 set transform-set siteB  
crypto map cienetvpn interface outside  
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

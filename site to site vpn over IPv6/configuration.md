![Image](https://github.com/silence-kai/IPsecVPN/blob/master/site%20to%20site%20vpn%20over%20IPv6/L2L-IPv6.png)
- FW_BJ(USG6305):  
  
ipv6  
  
acl ipv6 number 3000   
rule 1 permit ipv6 source FC00:1::0/64 destination FC00:2::0/64  
  
ipsec proposal tran1   
esp authentication-algorithm sha1  
esp encryption-algorithm aes-256  
  
ike proposal 10   
encryption-algorithm aes-256    
authentication-algorithm sha1  
authentication-method pre-share   
integrity-algorithm hmac-sha1  
  
ike peer b   
pre-shared-key xxxxxxxxx   
ike-proposal 10   
remote-address 2001::1   
  
ipsec policy map1 10 isakmp   
security acl ipv6 3000   
ike-peer b  
proposal tran1  
  
interface GigabitEthernet1 /0/2  
undo shutdown  
ipv6 enable   
ipv6 address FC00::2:1/64  
    
interface GigabitEthernet1 /0/0  
undo shutdown  
ipv6 enable    
ipv6 address 2002::1/64   
ipsec policy map1  
  
firewall zone trust   
set priority 85  
add interface GigabitEthernet1 /0/2  
  
firewall zone untrust   
set priority 5   
add interface GigabitEthernet1 /0/0  
  
ipv6 route-static 2001::1 64 2002::2   
ipv6 route-static FC00:1::0 64 2002::2   
  
security-policy  
rule name policy1   
source-zone trust  
destination-zone untrust  
source-address FC00:1::1 64  
destination-address FC00:2::1 64  
action permit  
rule name policy2  
source-zone untrust  
destination-zone trust  
source-address FC00:2::1 64  
destination-address FC00:1::1 64  
action permit  
rule name policy3  
source-zone local  
destination-zone untrust   
source-address 2001::1 64  
destination-address 2002::1 64  
action permit  
rule name policy4  
source-zone untrust  
destination-zone local  
source-address 2002::1 64  
destination-address 2001::1 64  
action permit  
  
nat-policy   
rule name NATIPv6  
source-zone trust  
egress-interface GigabitEthernet1 /0/0  
source-address range FC00:2::1 FC00:2::FFFF:FFFF:FFFF:FFFF  
action no-nat  

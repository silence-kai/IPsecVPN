![Image](https://github.com/silence-kai/IPsecVPN/blob/master/GRE%20tunnel%20over%20IPv6/GRE%20tunnel%20over%20IPv6.png)
- FW_BJ(USG6300):  
  
interface Tunnel1  
ipv6 enable  
ip address 1.1.1.1 255.255.255.0  
ipv6 address FC00:0:0:FFFF::2/64  
tunnel-protocol gre   
source GigabitEthernet1/0/1  
destination 125.69.76.132  
gre key cipher *******  
gre checksum   
service-manage ping permit  
  
ipv6 route-static FC00:0:0:1:: 64 Tunnel1  
ipv6 route-static FC00:0:0:AAAA:: 64 FC00:0:0:2::2  
ipv6 route-static FC00:0:0:BBBB:: 64 Tunnel1  
  
rule name IPv6-OUT-TO-IN  
source-zone untrust  
destination-zone trust  
source-address FC00:: 16  
destination-address FC00:: 16  
action permit  
rule name IPv6-OUT-TO-LOCAL  
source-zone untrust  
destination-zone local  
source-address 125.69.76.132 mask 255.255.255.255  
destination-address 112.86.129.194 mask 255.255.255.255  
action permit  

[!](Diagram1.png)
```
1. Ustalenie netmaski
  - dla 5000 urządzeń 255.255.224.0 czyli /19
  - dla 500 urządzeń 255.255.254.0 czyli /23
  
2. Ustalenie adresów sieci
  - lan1 172.22.144.0/23
  - lan2 172.22.128.0/19
  
3. Dodanie adresów ip (najlepiej w /etc/network/interfaces)
  - PC0
  enp0s8
    address 172.22.128.1
    netmask 255.255.224.0
  enp0s9
    address 172.22.144.1
    netmask 255.255.254.0
  - PC1
    address 172.22.144.2
    netmask 255.255.254.0
  - PC2
    address 172.22.128.2
    netmask 255.255.224.0

4. Ustalenie routingu (też interfaces)
  - PC1
    up ip route add default via 172.22.144.1
  - PC2
    up ip route add default via 172.22.128.1
    
5. Włączenie forwardowania pakietów na PC0
  echo 1 > /proc/sys/net/ipv4/ip_forward
  lub na stałe
  odkomentować net.ipv4.ip_forward=1 w /etc/sysctl.d/99-sysctl.conf
  
6. Dodanie reguły masquerade na PC0
  iptables -t nat -A POSTROUTING -s 172.22.128.0/19 -o enp0s3 -j MASQUERADE
  -||- dla -s 172.22.144.0/23
  
7. Dodanie adresów dns do PC1 i PC2 (opcjonalne)
  /etc/resolv.conf
  nameserver 1.1.1.1

```
1. Ustalenie netmaski
  - Suma stanowisk we wszystkich labolatoriach (+2 planowane) to 420 więc maska to /23
  - Lub każda kondygnacja ma własną sieć z netmaską, na każdym piętrze 140 stanowisk więc /24
  - W każdym labolatorium jest 35 stanowisk więc /26
  - Dla wifi min liczba urządzeń to 800 więc maska to /22
  
2. Ustalenie sieci
  188.156.220.160 to adres bazowy
  
  Router główny
  - piętro 0: 188.156.220.0/24
  - piętro 1: 188.156.221.0/24
  - piętro 2: 188.156.222.0/24
  - wifi: 188.156.224.0/22
  czy wifi ma być również na reszcie routerów?
      urządzenia pod wifi
        adresy z dhcp 188.156.224.1 - 188.156.227.254
  
  Router 0
  - 009
    10.0.9.0/26
  - 013
    10.0.13.0/26
  - 014
    10.0.14.0/26
  - planowane 017
    10.0.17.0/26
    
  Router 1
  - 115
    10.0.115.0/26
  - 116
    10.0.116.0/26
  - 117
    10.0.117.0/26
  - 122
    10.0.122.0/26
    
  Router 2
  - 201
    10.0.201.0/26
  - 202
    10.0.202.0/26
  - 203
    10.0.203.0/26
  - planowane 204
    10.0.204.0/26
    
3. Dodanie adresów IP
  PC0 RouterG
    eth0: internet
    eth1 (dla piętra 0)
      address 188.156.220.1
      netmask 255.255.255.0
    eth2 (dla piętra 1)
      address 188.156.221.1
      netmask 255.255.255.0
    eth3 (dla piętra 2)
      address 188.156.222.1
      netmask 255.255.255.0
    eth4 (dla wifi)
      address 188.156.224.1
      netmask 255.255.252.0
      
  PC1 Router0
    eth0 (czy to potrzebne?)
      address 188.156.220.2
      netmask 255.255.255.0
    eth1
      address 10.0.9.1 (nie można 0, cała numeracja w salach sie sypie?)
      netmask 255.255.255.192
    eth2
      address 10.0.13.1
      netmask 255.255.255.192
    eth3
      address 10.0.14.1
      netmask 255.255.255.192
    eth4
      address 10.0.17.1
      netmask 255.255.255.192
          
          PCty w sali 009 pod Routerem0
            adresy z dhcp 10.0.9.2 - 10.0.9.62 (czy przydziela kolejne po kolei, czy sobie losuje z tej puli?)
          PCty w sali 013 pod Routerem0
            adresy z dhcp 10.0.13.2 - 10.0.13.62
          PCty w sali 014 pod Routerem0
            adresy z dhcp 10.0.14.2 - 10.0.14.62
          PCty w sali 017 pod Routerem0
            adresy z dhcp 10.0.17.2 - 10.0.17.62

  PC2 Router1
    eth0
      address 188.156.221.2
      netmask 255.255.255.0
    eth1
      address 10.0.115.1
      netmask 255.255.255.192
    eth2
      address 10.0.116.1
      netmask 255.255.255.192
    eth3
      address 10.0.117.1
      netmask 255.255.255.192
    eth4
      address 10.0.122.1
      netmask 255.255.255.192
      
          PCty w sali 115 pod Routerem1
            adresy z dhcp 10.0.115.2 - 10.0.115.62
          PCty w sali 116 pod Routerem1
            adresy z dhcp 10.0.116.2 - 10.0.116.62
          PCty w sali 117 pod Routerem1
            adresy z dhcp 10.0.117.2 - 10.0.117.62
          PCty w sali 122 pod Routerem1
            adresy z dhcp 10.0.122.2 - 10.0.122.62
      
    PC3 Router2
      eth0
        address 188.156.222.2
        netmask 255.255.255.0
      eth1
        address 10.0.201.1
        netmask 255.255.255.192
      eth2
        address 10.0.202.1
        netmask 255.255.255.192
      eth3
        address 10.0.203.1
        netmask 255.255.255.192
      eth4
        address 10.0.204.1
        netmask 255.255.255.192
        
          PCty w sali 201 pod Routerem2
            adresy z dhcp 10.0.201.2 - 10.0.201.62
          PCty w sali 202 pod Routerem2
            adresy z dhcp 10.0.202.2 - 10.0.202.62
          PCty w sali 203 pod Routerem2
            adresy z dhcp 10.0.203.2 - 10.0.203.62
          PCty w sali 204 pod Routerem2
            adresy z dhcp 10.0.204.2 - 10.0.204.62
            
4. Uruchomienie dhcp + wpisanie dns
  
        
5. Ustalnie routingu
  PC0 RouterG
    czy automatycznie jest routing z wifi na internet

  PC1 Router0
    up ip rotue add default via 188.156.220.1
    
       PCty pod routerem0 w sali 009
         up ip route add default via 10.0.9.1
       PCty pod routerem0 w sali 013
         up ip route add default via 10.0.13.1
       PCty pod routerem0 w sali 014
         up ip route add default via 10.0.14.1
       PCty pod routerem0 w sali 017
         up ip route add default via 10.0.17.1
    
  PC2 Router1
    up ip rotue add default via 188.156.221.1
    
       PCty pod routerem1 w sali 115
         up ip route add default via 10.0.115.1 
       PCty pod routerem1 w sali 116
         up ip route add default via 10.0.116.1
       PCty pod routerem1 w sali 117
         up ip route add default via 10.0.117.1
       PCty pod routerem1 w sali 122
         up ip route add default via 10.0.122.1
  
  PC3 Router2
    up ip rotue add default via 188.156.222.1
    
       PCty pod routerem2 w sali 201
         up ip route add default via 10.0.201.1 
       PCty pod routerem2 w sali 202
         up ip route add default via 10.0.202.1
       PCty pod routerem2 w sali 203
         up ip route add default via 10.0.203.1
       PCty pod routerem2 w sali 204
         up ip route add default via 10.0.204.1
        
  urządzenia z wifi
    up ip rotue add default via 188.156.224.1
         
6. Włączenie forwardowania ip
  PC z routerami
  odkomentować net.ipv4.ip_forward=1 w /etc/sysctl.d/99-sysctl.conf

7. Włączenie reguły masquerade
  PC0 RouterG
  iptables -t nat -A POSTROUTING -s 188.156.220.0/24 -o enp0s3 -j MASQUERADE
                                 -s 188.156.221.0/24 -o enp0s3 -j MASQUERADE
                                 -s 188.156.222.0/24 -o enp0s3 -j MASQUERADE
                                 -s 188.156.224.0/24 -o enp0s3 -j MASQUERADE
  
  PC1 Router0
  iptables -t nat -A POSTROUTING -s 10.0.9.0/26 -o enp0s3 -j MASQUERADE
                                 -s 10.0.13.0/26 -o enp0s3 -j MASQUERADE
                                 -s 10.0.14.0/26 -o enp0s3 -j MASQUERADE
                                 -s 10.0.17.0/26 -o enp0s3 -j MASQUERADE
                                 
  PC2 Router1
  iptables -t nat -A POSTROUTING -s 10.0.115.0/26 -o enp0s3 -j MASQUERADE
                                 -s 10.0.116.0/26 -o enp0s3 -j MASQUERADE
                                 -s 10.0.117.0/26 -o enp0s3 -j MASQUERADE
                                 -s 10.0.122.0/26 -o enp0s3 -j MASQUERADE
  
  PC3 Router2
  iptables -t nat -A POSTROUTING -s 10.0.201.0/26 -o enp0s3 -j MASQUERADE
                                 -s 10.0.202.0/26 -o enp0s3 -j MASQUERADE
                                 -s 10.0.203.0/26 -o enp0s3 -j MASQUERADE
                                 -s 10.0.204.0/26 -o enp0s3 -j MASQUERADE
```

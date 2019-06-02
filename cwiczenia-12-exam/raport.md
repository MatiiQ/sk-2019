![](Diagram12.png)
```
1. Ustalenie netmaski
  - Suma stanowisk we wszystkich labolatoriach (+2 planowane) to 420 więc maska to /23
  - Lub każda kondygnacja ma własną sieć z netmaską, na każdym piętrze 140 stanowisk więc /24
  - Lub do routera głównego na daną sieć podłączone są tylko 2 PC - główny i na piętrze /30
  - W każdym labolatorium jest 35 stanowisk więc /26
  - Dla wifi min liczba urządzeń to 800 więc maska to /22
  
2. Ustalenie sieci
  188.156.220.160 to adres bazowy
  
  Router główny
  - piętro 0: 188.156.220.0/24 [maska /24 czy tylko na router główny i na piętrze]
  - piętro 1: 188.156.221.0/24
  - piętro 2: 188.156.222.0/24
  - wifi: 188.156.224.0/22
    czy podłączać wifi do reszty routerów? [teraz nie można pingować]
  
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
        
        Urządzenia pod wifi
          adresy z dhcp 188.156.224.2 - 188.156.227.254
      
  PC1 Router0
    eth0
      address 188.156.220.2
      netmask 255.255.255.0
    eth1
      address 10.0.9.1 [nie można 0, cała numeracja w salach sie sypie?]
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
          adresy z dhcp 10.0.9.2 - 10.0.9.62 [przydziela kolejne ale nie zapamiętuje]
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
  PC0 RouterG
    WIFI
      nano /etc/default/isc-dhcp-server 
        odkomentować ścieżkę do pliku config DHCPDv4_CONF
        dopisać interfejs INTERFACESv4="enp0s10"
      nano /etc/dhcp/dhcpd.conf - dopisać konfiguracje sieci :
        subnet 188.156.224.0 netmask 255.255.252.0 {
          range 188.156.224.2 188.156.227.254;
          option routers 188.156.224.1;
          option domain-name-servers 1.1.1.1, 1.0.0.1;
        }
      systemctl restart isc-dhcp-server
    
  PC1 Router0 (osobny subnet config dla każdego)
    SALA 009|013|014|017 
      nano /etc/default/isc-dhcp-server 
        odkomentować ścieżkę do pliku config DHCPDv4_CONF
        dopisać interfejs INTERFACESv4="enp0s8 enp0s9 enp0s10 enp0s11"
      nano /etc/dhcp/dhcpd.conf - dopisać konfiguracje sieci :
        subnet 10.0.9|13|14|17.0 netmask 255.255.255.192 {
          range 10.0.9|13|14|17.2 10.0.9|13|14|17.62;
          option routers 10.0.9|13|14|17.1;
          option domain-name-servers 1.1.1.1, 1.0.0.1;
        }
      systemctl restart isc-dhcp-server
  
  PC2 Router1 (osobny subnet config dla każdego)
    SALA 115|116|117|122
      nano /etc/default/isc-dhcp-server 
        odkomentować ścieżkę do pliku config DHCPDv4_CONF
        dopisać interfejs INTERFACESv4="enp0s8 enp0s9 enp0s10 enp0s11"
      nano /etc/dhcp/dhcpd.conf - dopisać konfiguracje sieci :
        subnet 10.0.115|116|117|122.0 netmask 255.255.255.192 {
          range 10.0.115|116|117|122.2 10.0.115|116|117|122.62;
          option routers 10.0.115|116|117|122.1;
          option domain-name-servers 1.1.1.1, 1.0.0.1;
        }
      systemctl restart isc-dhcp-server
  
  PC3 Router2 (osobny subnet config dla każdego)
    SALA 201|202|203|204
      nano /etc/default/isc-dhcp-server 
        odkomentować ścieżkę do pliku config DHCPDv4_CONF
        dopisać interfejs INTERFACESv4="enp0s8 enp0s9 enp0s10 enp0s11"
      nano /etc/dhcp/dhcpd.conf - dopisać konfiguracje sieci :
        subnet 10.0.201|202|203|204.0 netmask 255.255.255.192 {
          range 10.0.201|202|203|204.2 10.0.201|202|203|204.62;
          option routers 10.0.201|202|203|204.1;
          option domain-name-servers 1.1.1.1, 1.0.0.1;
        }
      systemctl restart isc-dhcp-server
        
5. Ustalnie routingu
  PC1 Router0
    up ip rotue add default via 188.156.220.1
    
       PCty pod routerem0 w sali 009|013|014|017 (już w dhcp server)
         up ip route add default via 10.0.9|13|14|17.1
    
  PC2 Router1
    up ip rotue add default via 188.156.221.1
    
       PCty pod routerem1 w sali 115|116|117|122 (już w dhcp server)
         up ip route add default via 10.0.115|116|117|122.1 
  
  PC3 Router2
    up ip rotue add default via 188.156.222.1
    
       PCty pod routerem1 w sali 201|202|203|204 (już w dhcp server)
         up ip route add default via 10.0.201|202|203|204.1 
        
  urządzenia z wifi (już w dhcp server)
    up ip rotue add default via 188.156.224.1
         
6. Włączenie forwardowania ip
  PC z routerami
  odkomentować net.ipv4.ip_forward=1 w /etc/sysctl.d/99-sysctl.conf

7. Włączenie reguły masquerade
  ipatables-save > /etc/iptables.up.rules (utworzenie pliku)
  w pliku /etc/iptables.up.rules dodać w *nat przed COMMIT 
  (lub normalnie z iptables i potem zasavować)
  
  PC0 RouterG
  -A POSTROUTING -s 188.156.220.0/24 -o enp0s3 -j MASQUERADE
                 -s 188.156.221.0/24 -o enp0s3 -j MASQUERADE
                 -s 188.156.222.0/24 -o enp0s3 -j MASQUERADE
                 -s 188.156.224.0/24 -o enp0s3 -j MASQUERADE
  
  PC1 Router0
  iptables -t nat -A POSTROUTING -s 10.0.9.0/26 -o enp0s3 -j MASQUERADE
                                 -s 10.0.13.0/26 -o enp0s3 -j MASQUERADE
                                 -s 10.0.14.0/26 -o enp0s3 -j MASQUERADE
                                 -s 10.0.17.0/26 -o enp0s3 -j MASQUERADE
                                 
  PC2 Router1
  -A POSTROUTING -s 10.0.115.0/26 -o enp0s3 -j MASQUERADE
                 -s 10.0.116.0/26 -o enp0s3 -j MASQUERADE
                 -s 10.0.117.0/26 -o enp0s3 -j MASQUERADE
                 -s 10.0.122.0/26 -o enp0s3 -j MASQUERADE
  
  PC3 Router2
  -A POSTROUTING -s 10.0.201.0/26 -o enp0s3 -j MASQUERADE
                 -s 10.0.202.0/26 -o enp0s3 -j MASQUERADE
                 -s 10.0.203.0/26 -o enp0s3 -j MASQUERADE
                 -s 10.0.204.0/26 -o enp0s3 -j MASQUERADE
  
  dodanie wpisu w /etc/network/interfaces (enp0s3 po adresacji)
    post-up iptables-restore < /etc/iptables.up.rules
    
```

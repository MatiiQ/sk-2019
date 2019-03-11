Podłączenie 2 maszyn do switcha
*******************************

1. Utworzenie sieci NAT w globalnych ustawieniach VirtualBox o adresie ip 172.16.100.0/24 z wyłączonym DHCP
2. Na obydwu maszynach dodanie adresów ip poleceniem ip addr add {ip} dev {nazwa}
    -1 maszyna 172.16.100.10
    -2 maszyna 172.16.100.11
3. Zresetowanie interfejsu sieciowego na obydwu maszynach poleceniem ip link set {nazwa} dev down a następnie ip link set {nazwa} dev up
4. Pingowanie odpowiadających adresów ip na obydwu maszynach
5. Włączenie httpchat na 1 maszynie
6. Wysyłanie wiadomości z 2 maszyny poleceniem curl -X POST -d '{"text": "Hello World"}' http://{ip_address}:8888/chat
7. Pobieranie wiadomości na 2 maszynie poleceniem curl -X POST -d '{"last_message_id":-1}' http://{ip_address}:8888/messages
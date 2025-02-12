1- Installation VM Linux Debian 
- En mode terminal sans bureau/interface graphique 
- root: install sudo
2- DHCP 
- Installation isc-dhcp-server
$sudo apt -y install isc-dhcp-server
- Désactiver dhcp local VMWare : Edit -> Virtual Network Editor -> décocher la case dhcp du vmnet de la VM (NAT ici)

- Configuration 
$sudo nano /etc/network/interfaces 
-> paramètre carte réseau ens33 => static
  ![inet static](https://github.com/user-attachments/assets/f7df2e88-63f7-4f2a-9b54-ea611f098a5f)

$ip a
-> vérifie que l'adresse IP est la bonne
  ![dhcp vm1 ip](https://github.com/user-attachments/assets/9938d1a8-c2fe-4508-9ce2-18eed22bbb45)

$sudo nano /etc/dhcp/dhcpd.conf 
-> paramètre conf dhcp
![dhcp conf 1](https://github.com/user-attachments/assets/68125600-f38d-4980-a27f-0d6b4572a1df)
![dhcp conf 2](https://github.com/user-attachments/assets/933806ae-64db-49ca-aa8b-9d1e3b7273a3)

$sudo nano /etc/default/isc-dhcp-server
-> uncomment les dhcp4 conf et dhcp4 pid et mettre en commentaire IPv6 et mettre l'interface ens33 pour l'IPv4 
![isc-dhcp-server](https://github.com/user-attachments/assets/2e1e84ca-62ee-4198-9ab1-699c11d892f2)

- Restart + status 
$sudo systemctl restart isc-dhcp-server 
$sudo systemctl status isc-dhcp-server
![isc status](https://github.com/user-attachments/assets/43aeca12-6749-41be-96b7-6430e53acbe8)

- ip a vm client
  ![dhcp vm2 ip](https://github.com/user-attachments/assets/7ae8535e-b497-40ab-9a70-c9484225c14a)
- erreur ?
-> vérifier la syntaxe, restart, ifdown/ifup, status

  
  3- FTP
- installation proftpd
  $sudo apt instal proftpd
  
- config proftpd
  $sudo nano /etc/proftpd/proftpd.conf
  
-> limit number of connexion 
-> limit login to registered user of allowed group
![Capture d’écran (77)](https://github.com/user-attachments/assets/8506fa22-da3b-4716-add2-dfe7a5bedc52)

-> disable anonymous login
![Capture d’écran (79)](https://github.com/user-attachments/assets/c8224af5-6064-4f11-9a42-991a7428b712)

-> change default port to 6500 for better security 
![Capture d’écran (76)](https://github.com/user-attachments/assets/988dc3fb-a0c8-48be-957c-9f4aebca7ef9)

-> enable ssh and sftp connexion
![Capture d’écran (78)](https://github.com/user-attachments/assets/805019e3-bbbe-40d5-b3da-88db80eaebd5)

  
- error ? 
-> check ssh connexion is configured correctly and enabled in conf file 
-> check defaultuser is correct 
-> check user is part of the group allowed to access the ftp server 
-> reload +status

  
4- SSH 
- install open-ssh server
  $sudo apt install openssh-server
- config
![ssh config](https://github.com/user-attachments/assets/e7fcafce-83be-4ca1-ac21-8dfe0c2eaa55)

  
5- DNS
- install bind
  $sudo apt install bind9 dnsutils
- config 
-> named.conf.local 
-> named.conf.option 
-> zones files: db.dns.ftp.com & reverse ip zone 0.0.16.171.in-addr.arpa

- restart + status bind
  
- test: nslookup & sftp laplateforme\@dns.ftp.com
  
- error? 
-> zone file + double check file path 
-> syntax 
-> named-checkzone "file"
ok 
-> restart + status 

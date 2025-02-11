1- Installation VM Linux Debian 
- En mode terminal sans bureau/interface graphique 
- root: install sudo
2- DHCP 
- Installation isc-dhcp-server
$sudo apt -y install isc-dhcp-server
- Désactiver dhcp local VMWare 
- Configuration 
$sudo nano /etc/network/interfaces 
-> paramètre carte réseau ens33 => static
  ![inet static](https://github.com/user-attachments/assets/f7df2e88-63f7-4f2a-9b54-ea611f098a5f)

$ip a
-> vérifie que l'adresse IP est la bonne
  
$sudo nano /etc/dhcp/dhcpd.conf 
-> paramètre conf dhcp 
$sudo nano /etc/default/isc-dhcp-server
-> uncomment les dhcp4 conf et dhcp4 pid et mettre en commentaire IPv6 et mettre l'interface ens33 pour l'IPv4 
- Restart + status 
$sudo systemctl restart isc-dhcp-server 
$sudo systemctl status isc-dhcp-server 
- ip a vm client 
- erreur ?
-> vérifier la syntaxe, restart, ifdown/ifup, status 
3- FTP
- installation proftpd 
- config proftpd
-> limit number of connexion 
-> limit login to registered user of allowed group 
-> disable anonymous login 
-> change default port to 6500 for better security 
-> enable ssh and sftp connexion 
- error ? 
-> check ssh connexion is configured correctly and enable in conf file 
-> check defaultuser is correct 
-> check user is part of the group allowed to access the ftp server 
-> reload +status
4- SSH 
- install open-ssh server 
- crypto key 
5- DNS
- install bind 
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

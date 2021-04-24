<br />
<p align="center">
    <img src="https://www.flaticon.com/svg/vstatic/svg/1135/1135251.svg?token=exp=1619188726~hmac=190e688ffed410020de195342192535b" alt="Logo" width="200" height="200">
  </a>

  <h3 align="center">Setting Server - Prak Jarkom</h3>

  <p align="center">
    Dibuat untuk mempermudah hidup!
    <br />
    <br />
    <a href="https://github.com/erik-cahya/Pratikum-Jarkom"><strong>↓ Explore the docs 😊 ↓ </strong></a>
    <br />
    <a href="#setting-file-server--samba-">Remote Server</a>
    ·
    <a href="#setting-file-server--samba-">File Server</a>
    ·
    <a href="#setting-ftp-server-vsftpd--filezilla">FTP Server</a>
    ·
    <a href="#setting-dns-server-bind-9">DNS Server</a>
  </p>
</p>


## Setting Remote Server ( SSH / Putty)
- install : `sudo apt-get install openssh-server`
- start ssh : `systemctl start ssh`
- cek openssh : `systemctl status ssh`
- Login putty menggunakan IP Linux



## Setting File Server ( SAMBA )
- install : `sudo apt-get install samba`
-  buat folder sharing : `mkdir sharing`
-  beri akses : `chmod 777 sharing`
-  buka file config : `nano /etc/samba/smb.conf`
-  Arahkan kursor ke paling bawah
-  tambahkan command berikut :
``` 
[sharing]
 path = /home/server/sharing 
 guest ok = yes 
 read only = no 
 create mode = 0764 
 directory mode = 0755 
```
-  save file : `ctrl x -> Y -> enter`
-  restart samba : `systemctl restart smbd`
-  tinggal akses ip server di client
-  beres :) 
 
 
 
## Setting FTP Server (vsftpd & filezilla)
-  Install `file zilla` di client / windows
-  Install vstpd di server : `sudo apt-get install vsftpd`
-  add user : `adduser erik`
-  enter enter ampe selesai
-  restart service : `systemctl restart vsftpd`
-  akses ftp di browser : `ftp://ip_server`



## Setting DNS Server (Bind 9)
-  Install bind9 : `sudo apt-get install bind9`
-  masuk ke direktori bind9 : `cd /etc/bind`
-  backup ke-3 file berikut : <br>
`cp named.conf.default-zones named.conf.default-zones.bck` <br>
`cp db.127 db.127.bck` <br>
`cp db.local db.local.bck`
-  edit file named conf : `nano named.conf.default-zones`
-  arahkan kursor ke paling bawah
-  tambahkan command berikut : 
```
"stikom.ac.id"{ 
type master;
file "etc/bind/db.stikom"; 
}; 

zone "60.168.192.in-addr.arpa"{
type master;
file "etc/bind/db.192";
};
```
-  copy lagi file berikut : <br>
`cp db.127 db.192` <br>
`cp db.local db.stikom`

-  edit db.192 : `nano db.192`
```
    isikan db.192 dengan domain
    
    1  IN  PTR  www.stikom.ac.id
    1  IN  PTR  ns.stikom.ac.id
```
-  save
-  edit db.stikom : `nano db.stikom`
```
    isikan db.stikom dengan IP Server
    
    ns  IN  A  192.168.60.10
    www  IN  A  192.168.60.10
```
-  save
-  cek named.conf.default.zone : `named-checkconf`
-  cek db yang sudah dibuat : `name-checkzone db.stikom db.192`
-  edit ip dns pada nameserver : `nano /etc/resolv.conf`
-  ganti nameservers dengan ip linux
-  restart service bind : `systemctl restart bind9`
-  tinggal nslookup & uji coba di client











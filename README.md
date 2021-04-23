<br />
<p align="center">
  <a href="https://github.com/othneildrew/Best-README-Template">
    <img src="https://www.flaticon.com/svg/vstatic/svg/1135/1135251.svg?token=exp=1619188726~hmac=190e688ffed410020de195342192535b" alt="Logo" width="200" height="200">
  </a>

  <h3 align="center">Setting Server - Prak Jarkom</h3>

  <p align="center">
    Dibuat untuk mempermudah hidup!
    <br />
    <a href="https://github.com/othneildrew/Best-README-Template"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/othneildrew/Best-README-Template">View Demo</a>
    ·
    <a href="https://github.com/othneildrew/Best-README-Template/issues">Report Bug</a>
    ·
    <a href="https://github.com/othneildrew/Best-README-Template/issues">Request Feature</a>
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











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

## Setting Web Server (Apache 2)
-  install apache2 : `sudo apt-get apache2`
-  restart service : `systemctl restart apache2`
-  cek status : `systemctl status apache2`
-  cek status pada client dengan mengetikkan ip server pada browser
```
"jika muncul web apache2 ubuntu default page" berarti sukses
```
-  buat 2 direktori untuk domain & subdomain : <br>
`mkdir /var/www/erik (folder domain)` <br>
`mkdir /var/www/ns (folder sub-domain)`

-  Masuk ke dalam folder conf : `cd /etc/apache2/sites-available/`
-  salin 2 file berikut ke dalam folder domain dan subdomain <br>
`cp 000-default.conf erik.conf` <br>
`cp 000-default.conf ns.conf`

-  konfigurasi file conf pertama berikut : `nano erik.conf`
	```
        * hapus pada bagian #ServerName localhost = ServerName erik.stikom (nama domain)
        * pada bagian ServerAdmin = ServerAdmin webmaster@erik.stikom
        * pada bagian DocumentRoot = DocumentRoot /var/www/erik (folder domain)
	```
- save
- konfigurasi file conf kedua berikut : `nano ns.conf`
	```
        * hapus pada bagian #ServerName localhost = ServerName ns.erik.stikom (nama sub-domain)
        * pada bagian ServerAdmin = ServerAdmin webmaster@erik.stikom
        * pada bagian DocumentRoot = DocumentRoot /var/www/ns (folder sub-domain)
	```
- save
- masuk ke folder stikom : `cd /var/www/erik`
- tambahkan file html : `nano index.html`
- isikan dengan script html berikut (bebas juga boleh sih )
```
<html>
<head>
        <title>Ini adalah site dari erik.stikom</title>
</head>
<body>
        <h1>Ini adalah page site erik.stikom</h1>
</body
</html>
```
- save
- masuk ke folder ns : `cd /var/www/ns`
- tambahkan file html : `nano index.html`
- isikan dengan script html berikut (bebas juga boleh sih ) 
```
<html>
<head>
        <title>Ini adalah site dari ns.erik.stikom</title>
</head>
<body>
        <h1>Ini adalah page site ns.erik.stikom</h1>
</body
</html>
```
- save
- aktifkan kedua konfigurasi yang dibuat : <br>
`a2ensite erik.conf`<br>
`a2ensite ns.conf`<br>
- nonaktifkan konfigurasi default : `a2dissite 000-default.conf`
- restart apache : `systemctl restart apache2`
- pengujian :
```
    buka browser dan ketikkan alamat domain 1 & 2
```









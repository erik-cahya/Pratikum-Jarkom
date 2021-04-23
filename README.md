### Setting Server

### Setting Remote Server ( SSH / Putty )
1. install : `sudo apt-get install openssh-server`
2. start ssh : `systemctl start ssh`
3. cek openssh : `systemctl status ssh`
4. Login putty menggunakan IP Linux

### Setting File Server ( SAMBA )
1. install : `sudo apt-get install samba`
2. buat folder sharing : `mkdir sharing`
3. beri akses : `chmod 777 sharing`
4. buka file config : `nano /etc/samba/smb.conf
5. Arahkan kursor ke paling bawah
6. tambahkan command berikut : <br>
``` 
[sharing]
 path = /home/server/sharing 
 guest ok = yes 
 read only = no 
 create mode = 0764 
 directory mode = 0755 
```
7. save file : `ctrl x -> Y -> enter`
8. restart samba : `systemctl restart smbd`
9. tinggal akses ip server di client
10. beres :) 
 
### Setting FTP Server (vsftpd & filezilla)
1. Install `file zilla` di client / windows
2. Install vstpd di server : `sudo apt-get install vsftpd`
3. add user : ` adduser erik`
4. enter enter ampe selesai
5. restart service : `systemctl restart vsftpd`
6. akses ftp di browser : `ftp://ip_server`

### Setting DNS Server (Bind 9)
1. Install bind9 : `sudo apt-get install bind9`
2. masuk ke direktori bind9 : `cd /etc/bind`
3. backup ke-3 file berikut : <br>
`cp named.conf.default-zones named.conf.default-zones.bck` <br>
`cp db.127 db.127.bck` <br>
`cp db.local db.local.bck` <br>
4. edit file named conf : `nano named.conf.default-zones`
5. arahkan kursor ke paling bawah
6. tambahkan command berikut : 
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
  
7. copy lagi file berikut : <br>
`cp db.127 db.192` <br>
`cp db.local db.stikom`












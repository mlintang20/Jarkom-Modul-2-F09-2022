# Jarkom-Modul-2-F09-2022

## Anggota Kelompok

<table>
    <tr>
        <th>NRP</th>
        <th>Nama</th>
    </tr>
    <tr>
        <td>Muhammad Lintang Panjerino</td>
        <td>5025201045</td>
    </tr>
    <tr>
        <td>Sayid Ziyad Ibrahim Alaydrus</td>
        <td>5025201147</td>
    </tr>
    <tr>
        <td>Wahyu Tri Saputro</td>
        <td>5025201217</td>
    </tr>
<table>

## NO 11.

### Akan tetapi, pada folder /public, Loid ingin hanya dapat melakukan directory listing saja.
    
### **Jawab :**
    
Untuk melakukan directory listing pada folder /public, di dalam file konfigurasi /etc/apache2/sites-available/eden.wise.f09.com bisa ditambahkan baris seperti berikut :

```
 <Directory /var/www/eden.wise.f09.com/public>
    Option +Indexes
 </Directory>
```
    
Setelah itu, kita bisa melakukan lynx domain www.eden.wise.f09.com/public dan akan mendapatkan hasil seperti berikut :
    
Gambar
    
## NO 12.
    
### Tidak hanya itu, Loid juga ingin menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache.
    
### **Jawab :**
    
Untuk mengganti error kode pada apache, maka file konfigurasi bisa ditambahkan baris seperti berikut :
    
`ErrorDocument 404 /error/404.html`
    
 Setelah itu, kita bisa melakukan lynx domain yang salah, maka akan mendapatkan hasil seperti berikut :   
    
 Gambar
    
 ## NO. 13
    
 ### Loid juga meminta Franky untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.eden.wise.yyy.com/public/js menjadi www.eden.wise.yyy.com/js.
    
 ### **Jawab :**
    
 Untuk membuat konfigurasi virtual host, maka hanya perlu menambahkan baris berikut di file konfigurasi /etc/apache2/sites-available/eden.wise.f09.com :
    
 `Alias "/js" "/var/www/eden.wise.f09.com/public/js"`   
   
Saat mencoba melakukan lynx www.eden.wise.f09.com/js, maka hasilnya akan sama dengan melakukan lynx www.eden.wise.f09.com/public/js.
    
Gambar
    
## NO. 14
    
### Loid meminta agar www.strix.operation.wise.yyy.com hanya bisa diakses dengan port 15000 dan port 15500.
    
### **Jawab :**
    
Pertama - tama, buat situs baru yaitu www.strix.operation.wise.f09.com dengan mengcopy paste template config 0000-default.conf yang ada di /etc/apache2/sites-available dengan :
    
```
 cd /etc/apache2/sites-available/
 cp 0000.default.conf strix.operation.wise.f09.com.conf
```
    
 Masuk ke dalam konfigurainya, dan lakukan beberapa edit dengan menyetting port menjadi 15000 dan 15500, dan sesuaikan ServerName, DocumentRoot, dan ServerAlias.
    
 ```
  <VirtualHost *:15000 *:15500>
  ServerName strix.operation.wise.f09.com
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/strix.operation.wise.f09.com
  ServerAlias www.strix.operation.wise.f09.com

  ... 
 ```
      
![Screenshot Soal 14a](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_14a.png)    
    
Lalu, setting port listen dengan baris berikut di /etc/apache2/ports.conf :
      
```
...
Listen 15000
Listen 15500      
...
```    
      
![Screenshot Soal 14b](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_14b.png)     
      
Aktifkan situs dan restart apache2
      
```
a2ensite strix.operation.wise.f09.com
service apache2 restart
```
      
Kemudian, buat dokumen root dengan membuat folder/direktori di /var/www       
      
`mkdir /var/www/strix.operation.wise.f09.com`
      
Download zip untuk soal strix.operation.wise di drive yang sudah disediakan, isi root dengan file yang ada di zip tersebut
      
```
wget https://drive.google.com/file/d/1bgd3B6VtDtVv2ouqyM8wLyZGzK5C9maT/view?usp=share_link  
      
unzip strix.operation.wise.zip      
cp -R strix.operation.wise/* /var/www/strix.operation.wise.f09.com
```
      
Kemudian lynx domain di Eden.
      
```
lynx strix.operation.wise.f09.com:15000
lynx strix.operation.wise.f09.com:15500
```      
      
![Screenshot Soal 14c](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_14c.png)    
      
## NO. 15
      
### Dengan autentikasi username Twilight dan password opStrix dan file di /var/www/strix.operation.wise.yyy.com.
      
### **Jawab :**
      
Buka konfigurasi strix.operation.wise.f09.com.conf di /etc/apache2/sites-available dan tambahkan edit konfigurasi dengan menambahkan baris autentikasi berikut :
      
```
...
<Directory "/var/www/strix.operation.wise.f09.com">
    AuthType Basic
    AuthName "Restricted Content"
    AuthUserFile /etc/apache2/.htpasswd
    Require valid-user
</Directory>
...
```

![Screenshot Soal 15a](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_15a.png)         
      
Buat username dan password menggunakan htpasswd
      
`htpasswd -c /etc/apache2/.htpasswd Twilight`
      
Setelah itu password akan diminta untuk akun Twilight. Apabila berhasil dalam membuat password, maka restart apache2
      
`service apache2 restart`
      
Test lynx di SSS
      
```
lynx strix.operation.wise.f09.com:15000
lynx strix.operation.wise.f09.com:15500
```
      
Maka hasilnya akan seperti ini :

![Screenshot Soal 15b](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_15b.png)
      
![Screenshot Soal 15c](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_15c.png) 
      
![Screenshot Soal 15d](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_15d.png)  
      
![Screenshot Soal 15e](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_15e.png)      

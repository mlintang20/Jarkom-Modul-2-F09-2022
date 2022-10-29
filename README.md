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

## NO 1.

### Semua node terhubung pada router Ostania, sehingga dapat mengakses internet 

pertama kita membuat topologi seperti yang ada pada soal.dd

[!https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/ea97bf3a5d69b25325d1a53da730192b2c348f33/img/no_1a.png]
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
    
Lalu, setting port listen dengan baris berikut di /etc/apache2/ports.conf :
      
```
...
Listen 15000
Listen 15500      
...
```      
      
Aktifkan situs dan restart apache2
      
```
a2ensite strix.operation.wise.f09.com
service apache2 restart
```
      
Kemudian, buat dokumen root dengan membuat folder/direktori di /var/www       
      
`mkdir /var/www/strix.operation.wise.f09.com`

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

Gambar

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

pertama kita membuat topologi seperti yang ada pada soal.

![no 1](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_1a.png)

lalu untuk mengecheck apakah semua node sudah terkoneksi internet , kita dapat mencoba dengan meng-ping google.com.

### wise
![wise](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_1b.png)
### Berlint
![Berlint](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_1c.png)
### Eden
![Eden](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_1d.png)
### SSS
![SSS](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_1e.png)
### garden
![GARDEN](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_1f.png)




## NO 2.

### Untuk mempermudah mendapatkan informasi mengenai misi dari Handler, bantulah Loid membuat website utama dengan akses wise.yyy.com dengan alias www.wise.yyy.com pada folder wise.

pertama kita tambahkan 'wise.F09.com' pada file `/etc/bind/named.conf.local`

lalu memodifikasi config pada `/etc/bind/wise/wise.f09.com` Menggunakan NS untuk mendelegasikan zone yang telah dibuat pada wise.F09.com, kemudian domain dipetakan pada IP Eden. lalu menggunakan Cname untuk membuat alias website www.F09.com. berikut adalah hasilnya:

![hasil](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_2b.png)

berikut adalah hasilnya setelah dicoba:

![hasil](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_2c.png)


## NO 3.
### Setelah itu ia juga ingin membuat subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden

Memodifikasi config pada file `/etc/bind/wise/wise.f09.com`. Menggunakan NS untuk mendelegasikan zone yang telah dibuat pada eden.wise.f09com. Untuk membuat alias www.eden.wise.f09.com menggunakan CNAME. Berikut adalah modifikasi config yang telah dilakukan:


![3.a](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_3a.png)

tes melakukan ping ke `eden.wise.F09.com` , dan dapat dilihat berhasil:
![hasil](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_3b.png)

## NO 6.

### Karena banyak informasi dari Handler, buatlah subdomain yang khusus untuk operation yaitu operation.wise.yyy.com dengan alias www.operation.wise.yyy.com yang didelegasikan dari WISE ke Berlint dengan IP menuju ke Eden dalam folder operation

### **Jawab :**

#### I. Konfigurasi pada _WISE_

- Pada _WISE_, edit file **/etc/bind/wise/wise.F09.com** dan ubah menjadi seperti pada gambar di bawah

![NO6](img/no_6a.png)

- Kemudian pada file **/etc/bind/named.conf.options** comment `dnssec-validation auto;` dan tambahkan `allow-query{any;};`

![NO6](img/no_6b.png)

- Kemudian restart bind9

```
service bind9 restart
```

#### II. Konfigurasi pada _Berlint_

- Pada file **/etc/bind/named.conf.options** comment `dnssec-validation auto;` dan tambahkan `allow-query{any;};`

![NO6](img/no_6c.png)

- Pada file **/etc/bind/named.conf.local** tambahkan zone operation:

```
zone "operation.wise.F09.com" {
    type master;
    file "/etc/bind/operation/operation.wise.F09.com";
};
```

- Buat folder dengan nama **operation** dan di dalamnya buat file dengan nama **/etc/bind/operation/operation.wise.F09.com**

- Edit file **/etc/bind/operation/operation.wise.F09.com** menjadi seperti berikut:

![NO6](img/no_6d.png)

- Kemudian restart bind9

```
service bind9 restart
```

#### III. Testing

- Lakukan ping ke **operation.wise.F09.com** dan **www.operation.wise.F09.com** dari client SSS

![NO6](img/no_6e.png)

## NO 7.

### Untuk informasi yang lebih spesifik mengenai Operation Strix, buatlah subdomain melalui Berlint dengan akses strix.operation.wise.yyy.com dengan alias www.strix.operation.wise.yyy.com yang mengarah ke Eden

### **Jawab :**

- Membuat subdomain strix (strix.operation.wise.F09.com) dalam subdomain operation dan juga alias dari strix (www.strix.operation.wise.F09.com) dengan edit file **/etc/bind/operation/operation.wise.F09.com** menjadi seperti berikut:

![NO7](img/no_7a.png)

- Untuk testing, lakukan ping ke **strix.operation.wise.F09.com** dan **www.strix.operation.wise.F09.com** dari client SSS

![NO7](img/no_7b.png)

## NO 8.

### Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.wise.yyy.com. Pertama, Loid membutuhkan webserver dengan DocumentRoot pada /var/www/wise.yyy.com

### **Jawab :**

- Pada folder **/etc/apache2/sites-available/** buat file konfigurasi baru bernama **/etc/apache2/sites-available/wise.F09.com.conf** yang meng-copy dari file **/etc/apache2/sites-available/000-default.conf**. Masuk dulu ke folder **/etc/apache2/** `cd /etc/apache2` kemudian copy file `cp 000-default.conf wise.F09.com.conf`

- Kemudian pada file **/etc/apache2/sites-available/wise.F09.com.conf** tambahkan _SeverName_ dan _ServerAlias_ menjadi seperti berikut

![NO8](img/no_8a.png)

- Aktifkan file konfigurasi dengan menggunakan a2ensite: `a2ensite wise.F09.com.conf`

- Buat folder baru **/var/www/wise.F09.com/** seperti pada dengan posisi dan nama folder yang sama pada _DocumentRoot_ : `mkdir /var/www/wise.F09.com/`

- Download file-file yang dibutuhkan menggunakan `wget` kemudian unzip file-file itu menggunakan `unzip` dan pindahkan ke dalam folder **/var/www/wise.F09.com/**

![NO8](img/no_8b.png)

- Testing dengan melakukan lynx ke **www.wise.F09.com**

```
lynx www.wise.F09.com
```

![NO8](img/no_8c.png)

## NO 9.

### Setelah itu, Loid juga membutuhkan agar url www.wise.yyy.com/index.php/home dapat menjadi menjadi www.wise.yyy.com/home

### **Jawab :**

- Di server _Eden_ pada file **/etc/apache2/sites-available/wise.F09.com.conf** tambahkan alias **/home** jika url yang diakses adalah www.wise.F09.com/index.php/home

![NO9](img/no_9a.png)

- Testing dengan melakukan lynx ke **lynx www.wise.F09.com/home** dan hasil yang keluar akan sama dengan ketika mengakses **lynx www.wise.F09.com/index.php/home**

```
lynx www.wise.F09.com/home
```

![NO9](img/no_9b.png)

## NO 10.

### Setelah itu, pada subdomain www.eden.wise.yyy.com, Loid membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/eden.wise.yyy.com

### **Jawab :**

- Pada folder **/etc/apache2/sites-available/** buat file konfigurasi baru bernama **/etc/apache2/sites-available/eden.wise.F09.com.conf** yang meng-copy dari file **/etc/apache2/sites-available/000-default.conf**. Masuk dulu ke folder **/etc/apache2/** `cd /etc/apache2` kemudian copy file `cp 000-default.conf eden.wise.F09.com.conf`

- Kemudian pada file **/etc/apache2/sites-available/wise.F09.com.conf** tambahkan _SeverName_ dan _ServerAlias_ menjadi seperti berikut

![NO10](img/no_10a.png)

- Aktifkan file konfigurasi dengan menggunakan a2ensite: `a2ensite eden.wise.F09.com.conf`

- Buat folder baru **/var/www/eden.wise.F09.com/** seperti pada dengan posisi dan nama folder yang sama pada _DocumentRoot_ : `mkdir /var/www/eden.wise.F09.com/`

- Download file-file yang dibutuhkan menggunakan `wget` kemudian unzip file-file itu menggunakan `unzip` dan pindahkan ke dalam folder **/var/www/eden.wise.F09.com/**

![NO10](img/no_10b.png)

- Testing dengan melakukan lynx ke **www.wise.F09.com**

```
lynx www.wise.F09.com
```

![NO10](img/no_10c.png)

## NO 11.

### Akan tetapi, pada folder /public, Loid ingin hanya dapat melakukan directory listing saja.

### **Jawab :**

Untuk melakukan directory listing pada folder /public, di dalam file konfigurasi /etc/apache2/sites-available/eden.wise.f09.com bisa ditambahkan baris seperti berikut :

```
 <Directory /var/www/eden.wise.f09.com/public>
    Option +Indexes
 </Directory>
```

![Screenshot Soal 11a](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_11a.png)

Setelah itu, kita bisa melakukan lynx domain www.eden.wise.f09.com/public dan akan mendapatkan hasil seperti berikut :

![Screenshot Soal 11b](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_11b.png)

## NO 12.

### Tidak hanya itu, Loid juga ingin menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache.

### **Jawab :**

Untuk mengganti error kode pada apache, maka file konfigurasi bisa ditambahkan baris seperti berikut :

`ErrorDocument 404 /error/404.html`

![Screenshot Soal 12a](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_12a.png)

Setelah itu, kita bisa melakukan lynx domain yang salah, maka akan mendapatkan hasil seperti berikut :

![Screenshot Soal 12b](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_12b.png)

## NO. 13

### Loid juga meminta Franky untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.eden.wise.yyy.com/public/js menjadi www.eden.wise.yyy.com/js.

### **Jawab :**

Untuk membuat konfigurasi virtual host, maka hanya perlu menambahkan baris berikut di file konfigurasi /etc/apache2/sites-available/eden.wise.f09.com :

`Alias "/js" "/var/www/eden.wise.f09.com/public/js"`

![Screenshot Soal 13a](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_13a.png)

Saat mencoba melakukan lynx www.eden.wise.f09.com/js, maka hasilnya akan sama dengan melakukan lynx www.eden.wise.f09.com/public/js.

![Screenshot Soal 13b](https://github.com/mlintang20/Jarkom-Modul-2-F09-2022/blob/master/img/no_13b.png)

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

## Kendala yang Dialami

- Beberapa kali terjadi error dan lama untuk menemukan solusinya, ternyata perlu untuk restart bind9 `service bind9 restart`
- Beberapa kali terjadi error pada web server dan lama untuk menemukan solusinya, ternyata perlu untuk restart apache2 `service apache2 restart`
- Tidak bisa ping karena ada yang typo pada file-file konfigurasi
- Tidak bisa ping karena salah nameserver pada file **/etc/resolv.conf**

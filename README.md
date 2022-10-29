# Jarkom-Modul-2-D02-2022 
### Laporan Resmi Pengerjaan Sesi Lab Jaringan Komputer     
        
## Anggota Kelompok D02
| NRP | Nama | Kontribusi |
| :---:        |     :---:           | :---: |
| 5025201082   | Farrel Emerson      | 1,2,3,4,5    |
| 5025201087   | Daniel Hermawan     | 8,9,14,15     |
| 5025201003   | Rahmat Faris Akbar  |   6,7,10-13,16,17    | 

## Script
file `script.sh` pada root tiap node

- Ostania

    ```bash
    #!/bin/bash
    
    iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.16.0.0/16
    ```

- SSS

    ```bash
    #!/bin/bash
    echo nameserver 192.168.122.1 > /etc/resolv.conf
    apt-get update
    apt-get install lynx -y
    apt-get install nano -y
    echo nameserver 10.16.2.2 > /etc/resolv.conf
    echo nameserver 10.16.3.2 >> /etc/resolv.conf
    ```

- Garden

    ```bash
    #!/bin/bash
    echo nameserver 192.168.122.1 > /etc/resolv.conf
    apt-get install nano -y
    echo nameserver 10.16.2.2 > /etc/resolv.conf
    echo nameserver 10.16.3.2 > /etc/resolv.conf
    ```

- WISE

    ```bash
    #!/bin/bash
    echo nameserver 192.168.122.1 > /etc/resolv.conf
    apt-get update
    apt-get install bind9 -y
    apt-get install nano -y
    mkdir /etc/bind/wise
    cp named.conf.local /etc/bind/named.conf.local
    cp wise.d02.com /etc/bind/wise/wise.d02.com
    cp 2.16.10.in-addr.arpa /etc/bind/wise/2.16.10.in-addr.arpa
    service bind9 restart
    ```

- Berlint

    ```bash
    #!/bin/bash
    echo nameserver 192.168.122.1 > /etc/resolv.conf
    apt-get update
    apt-get install bind9 -y
    apt-get install nano -y
    mkdir /etc/bind/operation
    cp named.conf.local /etc/bind/named.conf.local
    cp operation.wise.d02.com /etc/bind/operation/operation.wise.d02.com
    service bind9 restart
    ```

- Eden

    ```bash
    #!/bin/bash
    echo nameserver 192.168.122.1 > /etc/resolv.conf
    apt-get install nano -y
    apt-get install bind9 -y
    apt-get install apache2 -y
    apt-get install php -y
    apt-get install libapache2-mod-php7.0
    service apache2 start
    htpasswd -b -c /etc/apache2/.htpasswd Twilight opStrix
    
    cp webserver/wise.d02.com.conf /etc/apache2/sites-available
    cp webserver/eden.wise.d02.com.conf /etc/apache2/sites-available
    cp webserver/strix.operation.wise.d02.com-15000.conf /etc/apache2/sites-available/
    cp webserver/strix.operation.wise.d02.com-15500.conf /etc/apache2/sites-available/
    a2ensite wise.d02.com
    a2ensite eden.wise.d02.com
    a2ensite strix.operation.wise.d02.com-15000.conf
    a2ensite strix.operation.wise.d02.com-15000.conf
    service apache2 restart
    cp -r wise /var/www/wise.d02.com
    cp -r eden.wise /var/www/eden.wise.d02.com
    cp -r strix.operation.wise /var/www/strix.operation.wise.d02.com
    ```

- Eden (hanya dijalankan sekali diawal)

    ```bash
    #!/bin/bash
    apt-get install wget -y
    apt-get install unzip -y
    wget 'https://docs.google.com/uc?export=download&id=1q9g6nM85bW5T9f5yoyXtDqonUKKCHOTV' -O 'eden.wise.zip'
    wget 'https://docs.google.com/uc?export=download&id=1bgd3B6VtDtVv2ouqyM8wLyZGzK5C9maT' -O 'strix.operation.wise.zip'
    wget 'https://docs.google.com/uc?export=download&id=1S0XhL9ViYN7TyCj2W66BNEXQD2AAAw2e' -O 'wise.zip'
    unzip eden.wise.zip
    unzip strix.operation.wise.zip
    unzip wise.zip
    ```

## Pembukaan Soal         
Twilight (〈黄昏 (たそがれ) 〉, <Tasogare>) adalah seorang mata-mata yang berasal dari negara Westalis. Demi menjaga perdamaian antara Westalis dengan Ostania, Twilight dengan nama samaran Loid Forger (ロイド・フォージャー, Roido Fōjā) di bawah organisasi WISE menjalankan operasinya di negara Ostania dengan cara melakukan spionase, sabotase, penyadapan dan kemungkinan pembunuhan. Berikut adalah peta dari negara Ostania:

![image](https://user-images.githubusercontent.com/99629909/198015180-4331b045-fbab-42e4-9655-df34e585c10f.png)
         
## Jawaban Modul          
         
## Soal 1         
WISE akan dijadikan sebagai DNS Master, Berlint akan dijadikan DNS Slave, dan Eden akan digunakan sebagai Web Server. Terdapat 2 Client yaitu SSS, dan Garden. Semua node terhubung pada router Ostania, sehingga dapat mengakses internet.    

### Jawaban Soal 1      
Kami membuat topologi terlebih dahulu sebagai berikut:    

![image](https://user-images.githubusercontent.com/99629909/198755079-6445186b-358f-4467-b302-7210cad8eddf.png)
Kemudian yang kami lakukan adalah melakukan konfigurasi pada setiap node yang ada:       
       
Edit network configuration tiap node
- Ostania

    ```
    auto eth0
    iface eth0 inet dhcp

    auto eth1
    iface eth1 inet static
      address 10.16.1.1
      netmask 255.255.255.0

    auto eth2
    iface eth2 inet static
      address 10.16.2.1
      netmask 255.255.255.0

    auto eth3
    iface eth3 inet static
      address 10.16.3.1
      netmask 255.255.255.0
    ```

- SSS

    ```
    auto eth0
    iface eth0 inet static
      address 10.16.1.2
      netmask 255.255.255.0
      gateway 10.16.1.1
    ```

- Garden

    ```
    auto eth0
    iface eth0 inet static
      address 10.16.1.3
      netmask 255.255.255.0
      gateway 10.16.1.1
    ```

- WISE

    ```
    auto eth0
    iface eth0 inet static
      address 10.16.2.2
      netmask 255.255.255.0
      gateway 10.16.2.1
    ```

- Berlint

    ```
    auto eth0
    iface eth0 inet static
      address 10.16.3.2
      netmask 255.255.255.0
      gateway 10.16.3.1
    ```

- Eden

    ```
    auto eth0
    iface eth0 inet static
      address 10.16.3.3
      netmask 255.255.255.0
      gateway 10.16.3.1
    ```

ketik `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.16.0.0/16` pada node Ostania

Masukan name server pada tiap node

`echo nameserver 192.168.122.1 > /etc/resolv.conf`

Lakukan `ping google.com` pada setiap node

![image](https://user-images.githubusercontent.com/99629909/198755645-d91e277c-d5d4-42c3-9c6a-9884b3183e53.png)
![image](https://user-images.githubusercontent.com/99629909/198755674-4b21e20f-cbe2-4a34-9f7f-4d588a26fefc.png)
![image](https://user-images.githubusercontent.com/99629909/198755702-7d719cc9-c6df-4e61-9baf-6dab07834c20.png)
![image](https://user-images.githubusercontent.com/99629909/198755724-6fdff08d-421a-4aff-9b72-4424e6f505de.png)
![image](https://user-images.githubusercontent.com/99629909/198755748-36bb488e-22e5-44e7-b797-b0578bb72e9e.png)
![image](https://user-images.githubusercontent.com/99629909/198755794-a8672e2a-168e-48d4-98fa-67d480dc8335.png)

## Soal 2
Untuk mempermudah mendapatkan informasi mengenai misi dari Handler, bantulah Loid membuat website utama dengan akses wise.yyy.com dengan alias www.wise.yyy.com pada folder wise

### Jawaban Soal 2
pada root WISE buat file dengan `nano named.conf.local` lalu masukkan

![image](https://user-images.githubusercontent.com/99629909/198827686-932b6819-3075-49b0-99cc-bb28cddf1a44.png)

copy file ke /etc/bind/named.conf.local

`cp named.conf.local /etc/bind/named.conf.local`

Selain itu di nomor 4, Berlint akan dijadikan DNS Slave. pada root Berlint buat file dengan `nano named.conf.local` sebagai slave

![image](https://user-images.githubusercontent.com/99629909/198827757-312bd55e-59f0-436d-9f60-b5a16635b201.png)

copy file ke /etc/bind/named.conf.local

`cp named.conf.local /etc/bind/named.conf.local`

pada node WISE buat folder wise dalam /etc/bind

`mkdir /etc/bind/wise`

copy file db.local pada path /etc/bind ke root WISE

`cp /etc/bind/db.local wise.d02.com`

lalu edit isinya

![image](https://user-images.githubusercontent.com/99629909/198830255-05a7a7a5-8e84-430d-a792-22bcbabf78f8.png)

kemudian copy ke /etc/bind/wise/wise.d02.com

`cp wise.d02.com /etc/bind/wise/wise.d02.com`

lakukan `service bind9 restart`

pada node SSS dan Garden ganti nameserver pada `/etc/resolv.conf` menjadi IP WISE

`echo nameserver 10.16.2.2 > /etc/resolv.conf` dan `echo nameserver 10.16.3.2 >> /etc/resolv.conf`

lalu lakukan `ping www.wise.d02.com` pada SSS

![image](https://user-images.githubusercontent.com/99629909/198830518-b5b33ac5-779c-4661-999a-feb082d43fd9.png)


## Soal 3
Setelah itu ia juga ingin membuat subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden

### Jawaban Soal 3
Edit lagi file `wise.d02.com` pada root WISE dan tambahkan 2 line baru

![image](https://user-images.githubusercontent.com/99629909/198830534-49f4acf0-45dd-4539-8e43-6d4e841f95da.png)

kemudian copy ke /etc/bind/wise/wise.d02.com

`cp wise.d02.com /etc/bind/wise/wise.d02.com`

restart bind9, lakukan ping ke domain baru

`ping eden.wise.d02.com` atau `ping www.eden.wise.d02.com`

![image](https://user-images.githubusercontent.com/99629909/198830547-de3e0da7-952e-48dc-9c96-5b251820d6cd.png)
![image](https://user-images.githubusercontent.com/99629909/198830550-68553316-2e92-4992-98ba-9ce1d76b1df4.png)


## Soal 4
Buat juga reverse domain untuk domain utama

### Jawaban Soal 4
Edit lagi file `named.conf.local` pada root WISE dan tambahkan

![image](https://user-images.githubusercontent.com/99629909/198831470-998fb469-0abb-422a-a302-997afed62eb8.png)

kemudian copy lagi ke /etc/bind/named.conf.local

`cp named.conf.local /etc/bind/named.conf.local`

Copy lagi file pada `/etc/bind/db.local` ke root WISE

`cp /etc/bind/db.local 2.16.10.in-addr.arpa`

lalu ubah isinya menjadi

![image](https://user-images.githubusercontent.com/99629909/198830595-32eb2573-0ebe-4ba9-830d-9ee0bce92d46.png)

kemudian copy ke /etc/bind/wise/2.16.10.in-addr.arpa

`cp 2.16.10.in-addr.arpa /etc/bind/wise/2.16.10.in-addr.arpa`

restart bind9, lakukan `host -t PTR 10.16.2.2`

![image](https://user-images.githubusercontent.com/99629909/198831360-d3f1504b-57ff-46f4-8785-6d6db37c82ac.png)

## Soal 5
Agar dapat tetap dihubungi jika server WISE bermasalah, buatlah juga Berlint sebagai DNS Slave untuk domain utama

### Jawaban Soal 5
Sudah dibuat pada nomor 2, kemudian coba matikan node WISE

![image](https://user-images.githubusercontent.com/99629909/198831492-9c84aa37-80ce-41cf-b3b0-c77b6c7c9502.png)

dan lakukan `ping wise.d02.com` pada node SSS

![image](https://user-images.githubusercontent.com/99629909/198831504-4e61ed0f-4967-4d09-bb4f-befc8248ceae.png)


## Soal 6
Karena banyak informasi dari Handler, buatlah subdomain yang khusus untuk operation yaitu operation.wise.yyy.com dengan alias www.operation.wise.yyy.com yang didelegasikan dari WISE ke Berlint dengan IP menuju ke Eden dalam folder operation

### Jawaban Soal 6
Edit lagi file `wise.d02.com` pada root WISE dan tambahkan

![image](https://user-images.githubusercontent.com/99629909/198831518-75dcb8ee-e08a-424e-9543-1cb60b15bd6f.png)

lalu copy kembali ke /etc/bind/wise/wise.d02.com, Kemudian pada root Berlint edit file `named.conf.local` tambahkan

![image](https://user-images.githubusercontent.com/99629909/198831522-5166d915-0661-4a48-aa64-1927fbea3d2e.png)

lalu copy lagi ke /etc/bind/named.conf.local

Kemudian pada node Berlint buat direktori dengan nama operation

`mkdir /etc/bind/operation`

Copy lagi file pada `/etc/bind/db.local` ke root Berlint

`cp /etc/bind/db.local operation.wise.d02.com`

lalu ubah file menjadi

![image](https://user-images.githubusercontent.com/99629909/198831527-aaad5b2f-d7d0-4d79-8005-d6a12f623e4e.png)

copy kan ke `/etc/bind/operation/operation.wise.d02.com`

`cp operation.wise.d02.com /etc/bind/operation/operation.wise.d02.com`

restart bind9 pada WISE dan Berlint, lakukan ping ke `operation.wise.d02.com` dan `www.operation.wise.d02.com`

![image](https://user-images.githubusercontent.com/99629909/198831534-cea389b1-04f3-43e3-8959-7eef6550a6a7.png)


## Soal 7
Untuk informasi yang lebih spesifik mengenai Operation Strix, buatlah subdomain melalui Berlint dengan akses strix.operation.wise.yyy.com dengan alias www.strix.operation.wise.yyy.com yang mengarah ke Eden

### Jawaban Soal 7
Pada node Berlint edit lagi file `operation.wise.d02.com` pada root WISE dan tambahkan 2 line baru

![image](https://user-images.githubusercontent.com/99629909/198831550-b6c5ef14-ce98-4197-80ef-b86ef8dcbb3d.png)

copy kembali ke `/etc/bind/operation/operation.wise.d02.com`

restart bind9, lakukan ping ke `strix.operation.wise.d02.com` dan `www.strix.operation.wise.d02.com`

![image](https://user-images.githubusercontent.com/99629909/198831556-3556ed0b-1cec-4e98-8eae-1a7a4e5c9c28.png)
![image](https://user-images.githubusercontent.com/99629909/198831565-f442d3ec-f40e-43a0-b9a9-b0d12f84e17c.png)


## Soal 8
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.wise.yyy.com. Pertama, Loid membutuhkan webserver dengan DocumentRoot pada /var/www/wise.yyy.com

### Jawaban Soal 8
Pada script di node Eden (hanya dijalankan sekali diawal)
```bash
    apt-get install wget -y
    apt-get install unzip -y
    wget 'https://docs.google.com/uc?export=download&id=1q9g6nM85bW5T9f5yoyXtDqonUKKCHOTV' -O 'eden.wise.zip'
    wget 'https://docs.google.com/uc?export=download&id=1bgd3B6VtDtVv2ouqyM8wLyZGzK5C9maT' -O 'strix.operation.wise.zip'
    wget 'https://docs.google.com/uc?export=download&id=1S0XhL9ViYN7TyCj2W66BNEXQD2AAAw2e' -O 'wise.zip'
    unzip eden.wise.zip
    unzip strix.operation.wise.zip
    unzip wise.zip
```

Edit file `/etc/bind/wise/wise.d02.com` ubah menjadi IP Eden

![image](https://user-images.githubusercontent.com/99629909/198831588-47178c7f-18cf-4bcf-81f4-ff842a2a28a0.png)

Install apache2 dan php di Eden, kemudian pada Eden copy `/etc/apache2/sites-available/000-default.conf` ke folder `webserver/wise.d02.com.conf` pada root Eden, sebelumnya buat dulu folder webserver-nya. Lalu edit isinya

![image](https://user-images.githubusercontent.com/99629909/198831598-cb96df65-d544-4775-a69c-37a9e12b1aa3.png)

lalu copykan ke /etc/apache2/sites-available/wise.d02.com.conf

`cp webserver/wise.d02.com.conf /etc/apache2/sites-available/wise.d02.com.conf`

lalu aktifkan konfigurasi dengan `a2ensite wise.d02.com`

kemudian restart apachenya

`service apache2 restart`

kemudian copy file yang tadi sudah di download ke dalam file `/var/www/`

```bash
    cp -r wise /var/www/wise.d02.com
    cp -r eden.wise /var/www/eden.wise.d02.com
    cp -r strix.operation.wise /var/www/strix.operation.wise.d02.com
```

Pada node SSS jalankan `lynx www.wise.d02.com`
![image](https://user-images.githubusercontent.com/99629909/198831614-f6495aad-9f02-43e9-9d2f-6875278bd7d0.png)


## Soal 9
Setelah itu, Loid juga membutuhkan agar url www.wise.yyy.com/index.php/home dapat menjadi menjadi www.wise.yyy.com/home

### Jawaban Soal 9
Pada root Eden edit file `webserver/wise.d02.com.conf`

![image](https://user-images.githubusercontent.com/99629909/198830667-9bff006a-b742-44a6-b5d3-126e291b7b4a.png)

lalu copy lagi ke /etc/apache2/sites-available/wise.d02.com.conf

`cp webserver/wise.d02.com.conf /etc/apache2/sites-available/wise.d02.com.conf`

restart apache, kemudian pada node SSS jalankan `lynx http://wise.d02.com/home`

![image](https://user-images.githubusercontent.com/99629909/198830673-b4c638fc-ffb5-4cb5-a01a-9826632ea0c6.png)


## Soal 10
Setelah itu, pada subdomain www.eden.wise.yyy.com, Loid membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/eden.wise.yyy.com

### Jawaban
Pada node Eden copy file `wise.d02.com.conf` pada folder webserver dengan nama `eden.wise.d02.com.conf`

`cp webserver/wise.d02.com.conf webserver/eden.wise.d02.com.conf`

lalu edit isinya

![image](https://user-images.githubusercontent.com/99629909/198830681-ecb85053-f6a9-48aa-8744-c9d0e0de56b5.png)

lalu copy ke /etc/apache2/sites-available/eden.wise.d02.com.conf

`cp webserver/eden.wise.d02.com.conf /etc/apache2/sites-available/eden.wise.d02.com.conf`

lalu aktifkan konfigurasi dengan `a2ensite eden.wise.d02.com`

kemudian restart apachenya

`service apache2 restart`

Pada node SSS jalankan `lynx www.eden.wise.d02.com`

![image](https://user-images.githubusercontent.com/99629909/198830695-03a6f790-fa8c-4896-aea3-b514a1acf193.png)


## Soal 11
Akan tetapi, pada folder /public, Loid ingin hanya dapat melakukan directory listing saja

### Jawaban Soal 11
Pada node Eden edit file `webserver/eden.wise.d02.com.conf`

![image](https://user-images.githubusercontent.com/99629909/198830712-10f87063-786c-427c-a3c9-f7f9ba98ed88.png)

copy lagi ke /etc/apache2/sites-available/eden.wise.d02.com.conf

restart apache, pada node SSS jalankan `lynx www.eden.wise.d02.com`

![image](https://user-images.githubusercontent.com/99629909/198830719-e05d2965-0203-4ac9-b6d0-10c965f766bc.png)


## Soal 12
Tidak hanya itu, Loid juga ingin menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache

### Jawaban Soal 12
Pada node Eden edit lagi file `webserver/eden.wise.d02.com.conf`

![image](https://user-images.githubusercontent.com/99629909/198830735-d96d9a08-8dff-49ac-80bb-53422657fe40.png)

copy lagi ke /etc/apache2/sites-available/eden.wise.d02.com.conf

restart apache, pada node SSS jalankan dengan akhiran sembarang

![image](https://user-images.githubusercontent.com/99629909/198830744-baf8ef09-22e5-445e-a2cd-028ff06f5dcc.png)

![image](https://user-images.githubusercontent.com/99629909/198830749-d5398717-ae4a-4145-9dda-0d448171f53d.png)


## Soal 13
Loid juga meminta Franky untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.eden.wise.yyy.com/public/js menjadi www.eden.wise.yyy.com/js

### Jawaban Soal 13
Pada node Eden edit lagi file `webserver/eden.wise.d02.com.conf`

![image](https://user-images.githubusercontent.com/99629909/198830763-463fc352-bdee-44d2-8f68-b965ba0701d0.png)

copy lagi ke /etc/apache2/sites-available/eden.wise.d02.com.conf

restart apache, jalankan pada node SSS

`lynx www.eden.wise.d02.com/js`

![image](https://user-images.githubusercontent.com/99629909/198830773-c81564d4-9ae5-46f4-8521-0223cf8959aa.png)

![image](https://user-images.githubusercontent.com/99629909/198830782-5a5b368a-860d-4d26-8ca5-e6c229a76e08.png)


## Soal 14
Loid meminta agar www.strix.operation.wise.yyy.com hanya bisa diakses dengan
port 15000 dan port 15500

### Jawaban Soal 14
Untuk port 15000, buat file config baru dengan nama `strix.operation.wise.d02.com-15000.conf`

`cp /etc/apache2/sites-available/000-default.conf webserver/strix.operation.wise.d02.com-15000.conf`

lalu edit filenya

![image](https://user-images.githubusercontent.com/99629909/198830794-43d51992-5c41-4664-ae8f-13c6ce588e37.png)

lalu copy ke `/etc/apache2/sites-available/`

`cp webserver/strix.operation.wise.d02.com-15000.conf /etc/apache2/sites-available/`

Buat juga untuk port 15500.

Pada file `/etc/apache2/ports.conf` tambahkan listen port 15000 dan 15500

![image](https://user-images.githubusercontent.com/99629909/198830829-a34eb2d8-5032-490f-8c6e-54adc2f84693.png)

a2ensite kedua config, `a2ensite strix.operation.wise.d02.com-15000.conf` dan `a2ensite strix.operation.wise.d02.com-15500.conf`

restart apache, jalankan pada node SSS

`lynx www.strix.operation.wise.d02.com:15000`/`lynx www.strix.operation.wise.d02.com:15500`

![image](https://user-images.githubusercontent.com/99629909/198830886-9f861727-130d-4915-9146-d9ec7f8aabd9.png)

![image](https://user-images.githubusercontent.com/99629909/198830923-bd510f5a-b041-4bd7-a42d-e27e2f151277.png)

![image](https://user-images.githubusercontent.com/99629909/198830951-a5319494-bc42-448e-9390-3838a52a94f5.png)


## Soal 15
dengan autentikasi username Twilight dan password opStrix dan file di /var/www/strix.operation.wise.yyy

### Jawaban Soal 15
bikin password dengan

`htpasswd -b -c /etc/apache2/.htpasswd Twilight  opStrix`

![image](https://user-images.githubusercontent.com/99629909/198830980-fc700cfd-559e-4283-b202-ca63c998eae7.png)

lalu edit config pada `strix.operation.wise.d02.com-15000.conf` dan `strix.operation.wise.d02.com-15500.conf`

![image](https://user-images.githubusercontent.com/99629909/198830986-904f1b9f-4b8b-439d-87d2-d0097674cd42.png)

restart apache, jalankan pada node SSS

`lynx www.strix.operation.wise.d02.com:15000`/`lynx www.strix.operation.wise.d02.com:15500`

![image](https://user-images.githubusercontent.com/99629909/198831000-7b3fe345-2ab8-42ad-9b95-98dcc4852fe5.png)


## Soal 16
setiap kali mengakses IP Eden akan dialihkan secara otomatis ke www.wise.yyy.com

### Jawaban Soal 16
Pada node Eden jalankan `a2enmod rewrite` untuk mengaktifkan module rewrite

kemudian restart apache `service apache2 restart`

buat file baru dengan perintah `nano /var/www/wise.d02.com/.htaccess`

isi dengan kode berikut
```
RewriteEngine On
RewriteBase /
RewriteCond %{HTTP_HOST} ^10\.16\.3\.3$
RewriteRule ^(.*)$ http://www.wise.d02.com/$1 [L,R=301]
```

![image](https://user-images.githubusercontent.com/99629909/198831016-58a5f654-534d-4dd4-ae78-65df3025a9f1.png)

Edit isi dari `/etc/apache2/sites-available/000-default.conf` menjadi

![image](https://user-images.githubusercontent.com/99629909/198831022-e2f0abd3-0b59-473c-b558-8bde11da4ff9.png)

Kemudian, restard apachenya dengan `service apache2 restart` dan coba jalankan `lynx 10.16.3.3` pada node SSS

![image](https://user-images.githubusercontent.com/99629909/198831027-44be94d1-497a-4cd4-9efb-3802845ea023.png)

dan didapatkan hasil sebagai berikut

![image](https://user-images.githubusercontent.com/99629909/198831034-ba985911-1599-4a17-93c8-75da911791c8.png)


## Soal 17
Karena website www.eden.wise.yyy.com semakin banyak pengunjung dan banyak modifikasi sehingga banyak gambar-gambar yang random, maka Loid ingin mengubah request gambar yang memiliki substring “eden” akan diarahkan menuju eden.png. Bantulah Agent Twilight dan Organisasi WISE menjaga perdamaian!

### Jawaban Soal 17
Pada node Eden jalankan `a2enmod rewrite` untuk mengaktifkan module rewrite

buat file baru dengan perintah `nano /var/www/eden.wise.d02.com/.htaccess`

isi dengan kode berikut
```
RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_URI} ^/public/images/(.*)eden(.*)
RewriteRule .* http://www.eden.wise.d02.com/public/images/eden.png [L,R=301
```

![image](https://user-images.githubusercontent.com/99629909/198831097-ef04c6cf-861f-4af7-b948-ed743355d0fc.png)

Edit isi dari `/etc/apache2/sites-available/000-default.conf` menjadi

![image](https://user-images.githubusercontent.com/99629909/198831101-09312d86-9ab3-4e1b-8416-1afb0c6aaf12.png)

kemudian restart apache `service apache2 restart`

Jalankan `lynx www.eden.wise.d02.com/public/images/not-eden.png`

dan didapatkan hasil sebagai berikut

![image](https://user-images.githubusercontent.com/99629909/198831114-419634f1-3cd8-4272-8941-32adf7fd2a66.png)

![image](https://user-images.githubusercontent.com/99629909/198831121-57056d12-fca2-4c3c-a95c-a351986fbc4e.png)

  
## Kendala
- Pada soal no 8 dan 9 masih error "Alert!: Unable to connect to remote host." yang kemungkinan besar dikarenakan kesalahan konfigurasi
- Pada soal no 14 dan 15 masih error yang kemungkinan besar dikarenakan kesalahan konfigurasi

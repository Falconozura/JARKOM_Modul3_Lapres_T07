# JARKOM_Modul3_Lapres_T07

Lapres Praktikum Jarkom Modul 3<br />
<br />
![kelompok](https://img.shields.io/badge/Kelompok-T07-00a69a)<br />
<br />
Anggota:<br />
- ![fikri](https://img.shields.io/badge/Fikri%20Haykal-05311840000006-blueviolet)<br />
- ![syarif](https://img.shields.io/badge/Fancista%20Syarif%20H.-05311840000027-blueviolet)<br />

## Pengerjaan Soal

# 1. Membuat topologi jaringan sesuai pada modul.

Pembahasan :
1. Melakukan rm pada kota sebelumnya yang ada dalam praktikum.
2. Menambahkan Kota Malang sebagai DNS server.
3. Untuk DHCP server digunakan Kota Tuban.
4. Untuk Proxy server digunakan Kota Mojokerto.
5. Menambahkan Kota Madiun, Gresik, Banyuwangi, dan Sidoarjo pada klien.

# 2. Setting Router SURABAYA agar menjadi DHCP relay antar DHCP server dan client.

Pembahasan :
1. Melakukan `apt-get install isc-dhcp-relay` pada Surabaya.
2. Lalu buka `etc/default/isc-dhcp-relay`.
3. Menambahkan konfigurasi `SERVERS="IP DHCP Tuban"`.
4. Menambahkan konfigurasi `INTERFACES="eth1 eth2 eth3"'. 
5. Restart service dengan command `service isp-dhcp-relay restart`.

# 3. Setting DHCP server (TUBAN) agar subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.

Pembahasan :
1. Download DHCP server menggunakan command `apt-get install isc-dhcp-server`.
2. Setting INTERFACES yang digunakan oleh TUBAN pada file `/etc/default/isc-dhcp-server` dengan `INTERFACES="eth0"`.
3. Mendeklarasikan subnet 2 dengan `subnet 10.151.73.152 netmask 255.255.255.248` pada `/etc/dhcp/dhcpd.conf`.
4. Lalu setting untuk subnet 1 sebagai berikut :
```
subnet 192.168.0.0 netmask 255.255.255.0 { 
range 192.168.0.10 192.168.0.100;
range 192.168.0.110 192.168.0.200;
option routers 192.168.0.1;
option broadcast-address 192.168.0.255;
option domain-name-servers 10.151.77.66, 202.46.129.2;
default-lease-time 300;
max-lease-time 300;
}
```

# 4. Setting DHCP server (TUBAN) agar subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70.

Pembahasan :
1. Buka `etc/dhcp/dhcp.conf` pada Tuban.
2. Menambahkan konfigurasi subnet seperti berikut :
```
192.168.0.0 netmask 255.255.255.0 {
range 192.168.1.50 192.168.1.70;
option routers 192.168.1.1;
option broadcast-address 192.168.1.255;
option domain-name-servers 10.151.77.66, 202.46.129.2;
default-lease-time 600; max-lease-time 600; }
```
3. Melakukan konfigurasi pada Banyuwangi dan Madiun dengan `ifconfig`

# 5. Setting DHCP server (TUBAN) agar client mendapatkan DNS MALANG dan DNS 202.46.129.2 dari DHCP

Pembahasan :
1. Buka `etc/dhcp/dhcp.conf` pada Tuban.
2. Melakukan konfigurasi `option-domain-servers` sebagai berikut : `10.151.77.66, 202.46.129.2`.

# 6. Setting DHCP server (TUBAN) agar subnet 1 mendapatkan peminjaman alamat IP selama 5 menit dan subnet 3 mendapatkan peminjaman alamat IP selama 10 menit.

Pembahasan :
1. Membuka etc/dhcp/dhcp.conf pada Tuban.
2. Mengubah `default-lease-time` dan `max-lease-time` pada subnet 1 dan 3.
3. Pada subnet 1 konfigurasinya adalah : `default-lease-time 300` (5menit).
4. Pada subnet 3 konfigurasinya adalah : `default-lease-time 600` (10menit).

# 7. Setting agar akses Proxy server (MOJOKERTO) memerlukan autentikasi dengan `user: userta_t07` dan password: `inipassw0rdta_t07`

Pembahasan :
1. Melakukan `apt-get install squid` pada proxy server, yaitu Mojokerto.
2. Mengecek apakah squid sudah berjalan dengan `service squid status`.
3, Melakukan `apt-get install apache2-utils`.
4. Membuat user dan passsword `htpasswd -c /etc/squid/passwd userta_t07 inipassw0rdta_t07`
5. Membuka `etc/squid/squid.conf`.
6. Mengecek  `etc/squid/passwd`.
7. Melakukan konfigurasi squid sebagai berikut :
```
http_port 8080 visible_hostname mojokerto
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access deny !USERS
```
# 8. Setting agar Proxy server (MOJOKERTO) hanya dapat diakses pada hari Selasa-Rabu pukul 13.00-18.00.

Pembahasan :
1. Membuat file baru `acl.conf` dalam folder squid dan buka filenya.
2. Menambahkan isi filenya dengan `acl WORK1 time TW 13:00-18:00`. Lalu save.
3. Buka file `squid.conf`
4. Menambahkan konfigurasi `http_access allow WORK1`.

# 9. Setting agar Proxy server (MOJOKERTO) hanya dapat diakses pada hari Selasa-Kamis pukul 21.00-09.00 keesokan harinya.

Pembahasan :
1. Membuka file `acl.conf` dalam folder squid.
2. Menambahkan isi `acl.conf` dengan :
```
acl DEADLINE1 time TWH 21:00-24:00
acl DEADLINE2 time TWH 00:00-09:00
```
Lalu save filenya
3. Membuka file `squid.conf`
4. Menambahkan konfigurasi berikut :
```
http_access allow DEADLINE1
http_access allow DEADLINE2
```

# 10. Setting Proxy server (MOJOKERTO) agar ketika mengakses google.com, maka akan diredirect menuju monta.if.its.ac.id.

Pembahasan :
1. Membuka file `acl.conf`.
2. Menambahkan `acl lan src all` dan `acl google dstdomain .google.com`. Lalu save.
3. Membuka file `squid.conf`
4. Menambahkan `deny_info http://monta.if.its.ac.id lan http_reply_access deny google lan http_access allow all`.

# 11. Setting Proxy server (MOJOKERTO) agar mengubah error page default squid menjadi halaman yang disediakan.

Pembahasan :
1. Mendownload file yang disediakan dengan `wget 10.151.36.202/error403.tar.gz`.
2. Mengextract `tar -xvf error403.tar.gz`.
3. Menuliskan konfigurasi
`mv /usr/share/squid/errors/English/ERR_ACCESS_DENIED usr/share/squid/errors/English/ERR_ACCESS_DENIDE cp -r ERR_ACCESS_DENIED /usr/share/squid/errors/English/ERR_ACCESS_DENIED`

# 12. Setting DNS server (MALANG) agar mengakses Proxy server (MOJOKERTO) dapat melalui domain janganlupa-ta.t07.pw dengan port 8080.

Pembahasan :
1. Menuliskan dalam kolom HTTP Proxy "janganlupa-ta.t07.pw" dengan port "8080"
2. Masuk kedalam folder `named.conf.local` pada Malang.
3. Membuka `etc/bind/named.conf.local`.
4. Menambahkan `zone "janganlupa-ta.t07.pw" { type master; file "/etc/bind/jarkom/janganlupa-ta.t07.pw"; }.
5. Membuka file `janganlupa-ta.t07.pw`.
6. Membuka `etc/bind/jarkom/janganlupa-ta.t07.pw` dan menambahkan IP Malang.

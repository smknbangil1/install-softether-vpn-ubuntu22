Berikut adalah tutorial cara instalasi SoftEther VPN Server di Ubuntu 22.04 :

### Langkah 1 – Instalasi SoftEther VPN

1. **Install paket yang dibutuhkan** dengan menjalankan perintah berikut:
   ```
   apt-get install build-essential gnupg2 gcc make -y
   ```

2. **Unduh paket SoftEther VPN Server** dengan perintah `wget`:
   ```
   wget http://www.softether-download.com/files/softether/v4.38-9760-rtm-2021.08.17-tree/Linux/SoftEther_VPN_Server/64bit_-_Intel_x64_or_AMD64/softether-vpnserver-v4.38-9760-rtm-2021.08.17-linux-x64-64bit.tar.gz
   ```

3. **Ekstrak paket yang diunduh**:
   ```
   tar -xvzf softether-vpnserver-v4.38-9760-rtm-2021.08.17-linux-x64-64bit.tar.gz
   ```

4. **Masuk ke direktori yang diekstrak**:
   ```
   cd vpnserver
   ```

5. **Build SoftEther VPN Server**:
   ```
   make
   ```

6. **Pindahkan direktori hasil build ke `/usr/local/`**:
   ```
   cd ..
   mv vpnserver /usr/local/
   ```

7. **Setel izin yang tepat** untuk direktori `vpnserver`:
   ```
   cd /usr/local/vpnserver/
   chmod 600 *
   chmod 700 vpnserver
   chmod 700 vpncmd
   ```

### Langkah 2 – Buat File Systemd untuk SoftEther

1. **Buat file service untuk SoftEther**:
   ```
   nano /etc/init.d/vpnserver
   ```

2. **Tambahkan skrip berikut** ke dalam file:
   ```sh
   #!/bin/sh
   # chkconfig: 2345 99 01
   # description: SoftEther VPN Server
   DAEMON=/usr/local/vpnserver/vpnserver
   LOCK=/var/lock/subsys/vpnserver
   test -x $DAEMON || exit 0
   case "$1" in
   start)
   $DAEMON start
   touch $LOCK
   ;;
   stop)
   $DAEMON stop
   rm $LOCK
   ;;
   restart)
   $DAEMON stop
   sleep 3
   $DAEMON start
   ;;
   *)
   echo "Usage: $0 {start|stop|restart}"
   exit 1
   esac
   exit 0
   ```

3. **Buat direktori `subsys`** dan setel izin eksekusi untuk file `vpnserver`:
   ```
   mkdir /var/lock/subsys
   chmod 755 /etc/init.d/vpnserver
   ```

4. **Mulai SoftEther VPN**:
   ```
   /etc/init.d/vpnserver start
   ```

5. **Tambahkan SoftEther VPN ke sistem agar berjalan saat reboot**:
   ```
   update-rc.d vpnserver defaults
   ```

### Langkah 3 – Konfigurasi SoftEther VPN Server

1. **Masuk ke direktori SoftEther VPN Server**:
   ```
   cd /usr/local/vpnserver
   ```

2. **Jalankan utilitas `vpncmd`** untuk mengakses konsol SoftEther VPN Server:
   ```
   ./vpncmd
   ```

3. Ketika diminta untuk memilih, ketik `1` dan tekan Enter untuk mengelola server VPN.

4. **Sambungkan ke konsol VPN Server** dengan menekan Enter tanpa memasukkan nama host atau alamat IP, dan ikuti petunjuk untuk menyelesaikan konfigurasi.

SoftEther VPN sekarang siap digunakan!

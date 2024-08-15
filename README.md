# 1. Remote Deploy-in-server
# 2. Update Server dan Instalasi Node.js & Yarn

 ```bash
sudo apt update
sudo apt upgrade -y
```
# Instal Node.js dan Yarn:

sudo apt install curl -y
curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt install -y nodejs
node -v
npm -v

curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update && sudo apt install yarn
yarn -v

# instal mysql Server
sudo apt install mysql-server -y
sudo systemctl start mysql
sudo systemctl enable mysql

# 3. Setup Database MySQL
sudo mysql_secure_installation
mysql -u root -p
CREATE DATABASE nama_database;
CREATE USER 'user_database'@'localhost' IDENTIFIED BY 'password_database';
GRANT ALL PRIVILEGES ON nama_database.* TO 'user_database'@'localhost';
FLUSH PRIVILEGES;
EXIT;


# 3. Clone Project dari GitHub
# Di dalam server, navigasikan ke direktori di mana Anda ingin menyimpan project (misalnya, di /home/magang):

cd /home/magang

git clone https://github.com/username/repository.git
cd repository

# 4. Set File .env

nano .env

# isi file .env
DB_NAME=nama_database
DB_USERNAME=user_database
DB_PASSWD=password_database
DB_HOST=localhost

URL_API=https://url.api.pusat
SCHEDULE_TIME='55 23 * * *'
PORT=5000

# 5. Instalasi Dependencies
yarn install

# 6. Setup Database MySQL
mysql -u root -p
CREATE DATABASE nama_database;
CREATE USER 'user_database'@'localhost' IDENTIFIED BY 'password_database';
GRANT ALL PRIVILEGES ON nama_database.* TO 'user_database'@'localhost';
FLUSH PRIVILEGES;
EXIT;

# manual jika diperlukan
USE nama_database;
CREATE TABLE data_kecamatan (
  id INT AUTO_INCREMENT PRIMARY KEY,
  uuid VARCHAR(255) NOT NULL,
  kecamatan VARCHAR(255) NOT NULL,
  jumlah_objek INT NOT NULL,
  baku BIGINT NOT NULL,
  pokok BIGINT NOT NULL,
  denda BIGINT,
  realisasi BIGINT NOT NULL,
  persentase_realisasi FLOAT NOT NULL
);

# Setup cron job agar berjalan di latar belakang secara otomatis:
crontab -e

# Tambahkan job Anda (sesuaikan perintah yarn start sesuai dengan entry point aplikasi):
@reboot cd /home/magang/repository && yarn start

# 8. Keluar dari Remote SSH
exit

# 9. Verifikasi Cron Job
grep CRON /var/log/syslog

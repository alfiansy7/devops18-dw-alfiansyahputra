## Deploy Wayshub - Appserver

1. Login terlebih dahulu menggunakan terminal
```bash
ssh al@103.189.234.111
``` 

2. Lalu lakukan update packages
```bash
sudo apt update
``` 
<img src="images/image002.png">

3. Kemudian instal Node Version Manager
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
exec bash
``` 
<img src="images/image003.png">

4. Lalu instal node versi 14
```bash
nvm install 14
``` 
<img src="images/image004.png">

5. Clone repo wayshub fe
```bash
git clone https://github.com/dumbwaysdev/wayshub-frontend
``` 
<img src="images/image005.png">

6. Masuk ke direktori wayshub fe dan instal dependensi nya
```bash
cd wayshub-frontend/
npm install
``` 
<img src="images/image006.png">

7. Ganti BaseURL pada api.js
```bash
nano src/config/api.js
``` 
menjadi
```bash
api.<nama>.studentdumbways.my.id
``` 
<img src="images/image007.png">

8. Instal pm2 secara global
```bash
npm install -g pm2
``` 
<img src="images/image008.png">

9. Buat pm2 ecosystem
```bash
pm2 init simple
``` 
<img src="images/image009.png">

10. Edit ecosystem.config.js menjadi seperti ini
```bash
nano ecosystem.config.js
``` 
<img src="images/image010.png">

11. Lalu jalankan Wayshub FE dengan PM2
```bash
pm2 start
``` 
<img src="images/image011.png">

12. Clone Wayshub BE pada direktori home
```bash
cd ~
git clone https://github.com/dumbwaysdev/wayshub-backend
``` 
<img src="images/image012.png">

13. Masuk ke direktori Wayshub BE dan instal
```bash
cd wayshub-backend/
npm install
``` 
<img src="images/image013.png">

14. Edit config.json sesuaikan dengan database yang akan kita buat
```bash
nano config/config.json
``` 
<img src="images/image014.png">

15. Buat ecosystem PM2
```bash
pm2 init simple
``` 
<img src="images/image015.png">

16. Jalankan wayshub BE dengan PM2
```bash
pm2 start
``` 
<img src="images/image016.png">

17. Instal MySQL Server
```bash
sudo apt install mysql-server -y
``` 
<img src="images/image017.png">

18. Lalu instal mysql secure installation
```bash
sudo mysql_secure_installation 
``` 
<img src="images/image018.png">

19. Menerapkan password validasi [Y]
<img src="images/image019.png">

20. Menerapkan low level password [0]
<img src="images/image020.png">

21. Menghapus anonymous users [Y]
<img src="images/image021.png">

22. Menerapkan remote db [N]
<img src="images/image022.png">

23. Menghapus db test [Y]
<img src="images/image023.png">

24. Reload privilege [Y]
<img src="images/image024.png">

25. Login ke mysql sebagai root
```bash
sudo mysql -u root
``` 
<img src="images/image025.png">

26. Membuat password root 
```bash
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'Bl4ckP1nk23!';
``` 
<img src="images/image026.png">

27. Membuat user baru dan dapat melakukan remote
```bash
CREATE USER 'alf'@'%' IDENTIFIED WITH mysql_native_password by 'Bl4ckP1nk23@';
``` 
<img src="images/image027.png">

28. Menambahkan hak akses user alf ke semua db dan tabel
```bash
GRANT ALL PRIVILEGES ON *.* TO 'alf'@'%';
``` 
<img src="images/image028.png">

29. Lalu instal Sequelize di global
```bash
npm install -g sequelize-cli
``` 
<img src="images/image029.png">

30. Masuk ke direktori Wayshub BE dan membuat db dengan bantuan Sequelize
```bash
cd wayshub-backend/
sequelize db:create
``` 
<img src="images/image030.png">

31. Kemudian migrate table Wayshub BE
```bash
sequelize db:migrate
``` 
<img src="images/image031.png">

32. Lalu restart PM2 Wayshub FE dan BE
```bash
pm2 start
cd ../wayshub-frontend/
pm2 start
``` 
<img src="images/image032.png">

[**Back**](../../README.md)

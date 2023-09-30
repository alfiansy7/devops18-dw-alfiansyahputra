# Docker

## 1. Gateway Server

1. Login ke gateway server terlebih dahulu
```bash
ssh username@ip
``` 

2. Lakukan instalasi docker menggunakan script bash berikut
```bash
sudo sh install-docker.sh
``` 
<img src="images/image002.png">

3. Install snapd dan certbot
```bash
sudo apt install snapd -y
sudo snap install --classic certbot -y
sudo ln -s /snap/bin/certbot /usr/bin/certbot
``` 
<img src="images/image003.png">

4. Membuat sertifikat domain creapy dan api.creapy
```bash
sudo certbot certonly --standalone -d creapy.studentdumbways.my.id 
sudo certbot certonly --standalone -d api.creapy.studentdumbways.my.id
``` 
<img src="images/image004.png">

5. Membuat config wayshub untuk nginx
```bash
nano wayshub.conf
``` 
<img src="images/image005.png">

6. Membuat direktori ssl sebagai volumes di docker
```bash
mkdir -p ssl/creapy.studentdumbways.my.id
mkdir -p ssl/api.creapy.studentdumbways.my.id
``` 
<img src="images/image006.png">

7. Lalu cek symlink file sertifikatnya
```bash
sudo ls -l /etc/letsencrypt/live/creapy.studentdumbways.my.id/
sudo ls -l /etc/letsencrypt/live/api.creapy.studentdumbways.my.id/
``` 
<img src="images/image007.png">

8. Copy file certifikatnya ke direktori ssl
```bash
sudo cp /etc/letsencrypt/archive/creapy.studentdumbways.my.id/fullchain1.pem ssl/creapy.studentdumbways.my.id/fullchain.pem
sudo cp /etc/letsencrypt/archive/creapy.studentdumbways.my.id/privkey1.pem ssl/creapy.studentdumbways.my.id/privkey.pem
sudo cp /etc/letsencrypt/archive/api.creapy.studentdumbways.my.id/fullchain1.pem ssl/api.creapy.studentdumbways.my.id/fullchain.pem
sudo cp /etc/letsencrypt/archive/api.creapy.studentdumbways.my.id/privkey1.pem ssl/api.creapy.studentdumbways.my.id/privkey.pem
``` 
<img src="images/image008.png">

9. Download, buat volums dan jalankan nginx di docker
```bash
docker run -d --name nginx -p 80:80 -p 443:443 -v ~/ssl:/etc/letsencrypt/live nginx
``` 
<img src="images/image009.png">

10. Copy config yang sudah kita buat tadi ke nginx yang ada di docker
```bash
docker cp wayshub.conf nginx:/etc/nginx/conf.d/wayshub.conf
``` 
<img src="images/image010.png">

11. Restart nginx
```bash
docker restart nginx
``` 
<img src="images/image011.png">

12. Cek https://creapy.studentdumbways.my.id/
<img src="images/image012.png">


## 2. App Server

13. Login ke appserver menggunakan ssh
```bash
ssh username@ip
``` 

14. Lakukan instalasi docker menggunakan script bash berikut
```bash
sudo sh install-docker.sh
``` 
<img src="images/image014.png">

15. Clone wayshub frontend dan backend
```bash
git clone https://github.com/dumbwaysdev/wayshub-frontend.git
git clone https://github.com/dumbwaysdev/wayshub-backend.git
``` 
<img src="images/image015.png">

16. Masuk ke direktori wayshub-frontend dan membuat Dockerfile
```bash
cd wayshub-frontend/
nano Dockerfile
``` 
<img src="images/image016.png">

17. Edit api.js pada wayshub-frontend
```bash
nano src/config/api.js
``` 
<img src="images/image017.png">

18. Buat image docker dengan nama wayshub-fe 
```bash
docker build -t zipian/wayshub-fe .
``` 
<img src="images/image018.png">

19. Masuk ke direktori wayshub-backend dan membuat Dockerfile
```bash
cd ~/wayshub-backend/
nano Dockerfile
``` 
<img src="images/image019.png">

20. Edit config.json pada wayshub-backend
```bash
nano config/config.json
``` 
<img src="images/image020.png">

21. Pull mysql untuk kebutuhan build wayshub-be
```bash
docker run -d --name mysql-temp -p 3306:3306 -e MYSQL_ROOT_PASSWORD=Jayamas mysql
``` 
<img src="images/image021.png">

22. Buat image docker dengan nama wayshub-be 
```bash
docker build -t zipian/wayshub-be .
``` 
<img src="images/image022.png">

23. Cek images yg sudah dibuat
```bash
docker images
``` 
<img src="images/image023.png">

24. Login ke docker hub
```bash
docker login 
``` 
<img src="images/image024.png">

25. Push image wayshub-fe dan wayshub-be
```bash
docker push zipian/wayshub-fe
docker push zipian/wayshub-be
``` 
<img src="images/image025.png">

26. Kembali ke home direktori dan membuat docker compose
```bash
cd ~
nano docker-compose.yml
``` 
<img src="images/image026.png">

27. Lalu jalankan docker compose
```bash
docker compose up -d
``` 
<img src="images/image027.png">

28. Buka website dan lalukan registrasi
```bash
https://creapy.studentdumbways.my.id/register
``` 
<img src="images/image028.png">

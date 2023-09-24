## Gateway NGINX

1. Login terlebih dahulu menggunakan terminal
```bash
ssh al@103.250.11.33
``` 

2. Lalu lakukan update packages
```bash
sudo apt update
``` 
<img src="images/image002.png">

3. Kemudian instal nginx
```bash
sudo apt install nginx -y
``` 
<img src="images/image003.png">

4. Membuat direktori config pada nginx
```bash
sudo mkdir /etc/nginx/wayshub
``` 
<img src="images/image004.png">

5. Buat fe.conf pada direktori wayshub-nginx
```bash
sudo nano /etc/nginx/wayshub/fe.conf
``` 
<img src="images/image005.png">

6. Buat juga be.conf pada direktori wayshub-nginx
```bash
sudo nano /etc/nginx/wayshub/be.conf
``` 
<img src="images/image006.png">

7. Tambahkan direktori wayshub pada nginx.conf
```bash
sudo nano /etc/nginx/nginx.conf
``` 
<img src="images/image007.png">

8. Restart nginx
```bash
sudo systemctl restart nginx
``` 
<img src="images/image008.png">

9. Instal snapd
```bash
sudo apt install snapd
``` 
<img src="images/image009.png">

10. Instal certbot menggunakan snapd
```bash
sudo snap install --classic certbot
``` 
<img src="images/image010.png">

11. Buat symlink certbot
```bash
sudo ln -s /snap/bin/certbot /usr/bin/certbot
``` 
<img src="images/image011.png">

12. Instal cerbot di nginx
```bash
sudo certbot --nginx
``` 
<img src="images/image012.png">

13. Masukan email
<img src="images/image013.png">

14. Setujui ToS [Y]
<img src="images/image014.png">

15. Subscribe newsletter [Y]
<img src="images/image015.png">

16. Instal ke 2 domain dengan cara kosongkan lalu enter
<img src="images/image016.png">

17. Membuat buat pembaruan otomatis
```bash
sudo certbot renew --dry-run
``` 
<img src="images/image017.png">

18. Lakukan percobaan dengan menggunakan https
<img src="images/image018.png">
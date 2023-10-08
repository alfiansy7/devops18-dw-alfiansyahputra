# Terraform

## Docker

1. Login terlebih dahulu menggunakan terminal
```bash
ssh appserver
``` 
<img src="images/image001.png">

2. Melakukan instalasi terraform menggunakan script berikut
```bash
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/hashicorp.list
apt update && apt install terraform -y
``` 
<img src="images/image002.png">

3. Membuat direktori terraform dan masuk ke direktori tersebut
```bash
mkdir terraform && cd terraform
``` 
<img src="images/image003.png">

4. Mebuat file main.tf dengan script berikut
```bash
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {}

resource "docker_image" "wayshub-fe" {
  name         = "zipian/wayshub-fe:latest"
  keep_locally = false
}

resource "docker_container" "wayshub-fe" {
  image = docker_image.wayshub-fe.image_id
  name  = "wayshub-fe"

  ports {
    internal = 3000
    external = 80
  }
}
``` 
<img src="images/image004.png">

5. Inisialisasi direktori tersebut
```bash
terraform init
``` 
<img src="images/image005.png">

6. Lakukan pengecekan detail apa saja yang akan dijalankan oleh terraform
```bash
terraform plan
``` 
<img src="images/image006.png">

7. Jalankan wayshub-fe
```bash
terraform apply
``` 
<img src="images/image007.png">

8. Cek apakah container tersebut berhasil dijalankan
```bash
docker ps -a
``` 
<img src="images/image008.png">

9. Cek menggunakan browser
```bash
http://103.175.220.130
``` 
<img src="images/image009.png">

10. Lalu untuk menghancurkan resource nya cukup dengan code berikut karna sudah kita set sebelumnya untuk keep_locally = false
```bash
terraform destroy
``` 
<img src="images/image010.png">

[**Back**](../../README.md)
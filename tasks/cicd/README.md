# CI/CD

## 1. Jenkins

1. Login ke appserver terlebih dahulu
```bash
ssh username@ip
``` 

2. Download dan jalankan jenkins pada docker
```bash
docker run -d --name jenkins -p 8080:8080 -p 8081:5000 --restart=on-failure jenkins/jenkins:lts-jdk11
``` 
<img src="images/image002.png">

3. Lihat password instalasinya
```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
``` 
<img src="images/image003.png">

4. Buka website jenkins nya dan paste password instalasinya
```bash
https://jenkins.creapy.studentdumbways.my.id/
``` 
<img src="images/image004.png">

5. Pilih *Select plugins to install*
<img src="images/image005.png">

6. Pastikan centang _SSH Build Agents_ pada *Distributed Builds* dan _SSH Agent_ pada *Build Features*
<img src="images/image006.png">

7. Tunggu hingga proses instalasi selesai
<img src="images/image007.png">

8. Masukan data admin user
<img src="images/image008.png">

9. Lalu set base url jenkins nya
<img src="images/image009.png">

10. Jenkins siap digunakan
<img src="images/image010.png">

11. Import ssh private key terlebih dahulu
```bash
Dashboard -> Manage Jenkins -> Credentials -> System -> Global credentials (unrestricted) -> Add credentials
``` 
<img src="images/image011.png">

12. Isikan data berikut
- Kind: SSH Username with pricate key
- ID: appserver (bisa di isi nama dari servernya)
- Username: alfiansy (username pada server)
- Ceklis *Treat username as secret* (agar username tidak bisa dilihat)
- Private Key: Paste private key
<img src="images/image012.png">

13. Ganti Git Host Key Verif ke *Accept first conn*
<img src="images/image013.png">

14. Clone repo private kita dan buat jenkinsfile
```bash
git clone git@github.com:alfiansy7/wayshub-frontend.git
cd wayshub-frontend
nano Jenkinsfile
``` 
```bash
def cred = 'appserver'
def server = 'alfiansy@al-appserver.studentdumbways.my.id'
def dir = '~/wayshub-frontend'
def img = 'zipian/wayshub-fe'
def branch = 'main'

pipeline{
    agent any
    stages{
        stage('docker compose down and pulling repo'){
            steps{
                sshagent([cred]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ~
                    docker compose down
                    cd ${dir}
                    git pull origin main
                    exit
                    EOF"""
                }
            }
        }
        stage('building docker image'){
            steps{
                sshagent([cred]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${dir}
                    docker build -t zipian/wayshub-fe .
                    exit
                    EOF"""
                }
            }
        }
        stage('docker compose up'){
            steps{
                sshagent([cred]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${dir}
                    docker compose up -d
                    exit
                    EOF"""
                }
            }
        }
        stage('push docker hub'){
            steps{
                sshagent([cred]){
                     sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${dir}
                    docker image push ${img}:latest
                    exit
                    EOF"""
                }
            }
        }
        stage('push notification'){
            steps{
                sshagent([cred]){
                    discordSend description: "wayshub-frontend:" + BUILD_ID, link: BUILD_URL, result: currentBuild.currentResult, scmWebUrl: '', title: 'Wayshub-Frontend', webhookURL: 'https://discord.com/api/webhooks/1157983493461114980/ACYgAga_6hQ-z7VVdNXoo--jOcUNknQWkaRcbm3hd1XZx72jY04hzoZZdZ0-YCAgcolU'
                }
            }
        }
    }
}
``` 
<img src="images/image014.png">

15. Push Jenkinsfile
```bash
git add .
git commit -m "add Jenkinsfile"
git push
``` 
<img src="images/image015.png">

16. Buat job baru 
- name: wayshub-fe
- categori: pipeline
- Ceklis *GitHub hook trigger*
- Gunakan *Pipeline script from SCM*
- Repo url: isikan ssh git
- Credentials: isikan server kita
- Branch: main
<img src="images/image016.png">

17. Tambahkan webhook pada github
<img src="images/image017.png">

18. Lakukan percobaan dengan menambah file pada repo dan push ulang
<img src="images/image018-1.png">
<img src="images/image018-2.png">
<img src="images/image018-3.png">

## 2. GitLabCI

19.


[**Back**](../../README.md)
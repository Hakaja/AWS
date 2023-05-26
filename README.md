# **Dokumentation für AWS, Mai 2023**  

## Git und CodeCommit

    cd
    cd environment/Praktikum/

    git config --global user.name "Paula Menski"
    git config --global user.email "paula.menski@disroot.com"

Führen Sie am Terminal die folgenden Befehle zum Konfigurieren des Hilfsprogramms für AWS CLI-Anmeldeinformationen für HTTPS-Verbindungen aus:

    git config --global credential.helper '!aws codecommit credential-helper $@'
    git config --global credential.UseHttpPath true
    
Klonen des Repositorys mit CodeCommit:

    git clone https://git-codecommit.eu-central-1.amazonaws.com/v1/repos/Praktikum-Paula

    cd environment/Praktikum/Praktikum-Paula
    git add -Add
    git commit -m "Commit"
    git push
    
## Git und Github

    cd 
    cd environment/Praktikum
    git config --global user.name "Paula Menski"
    git config --global user.email "paula.menski@disroot.com"
    git init
    
SSH PublicKey in Github Profil hinzufügen, danach Repo erstellenund SSH-URL Kopieren:

    ssh -T git@github.com
    git remote add origin "SSH-URL"
    git push -u origin master

### Cloud9 mit CodeCommit synchronisieren

[Referenz-1 Integration von AWS Cloud9 in AWS CodeCommit](https://docs.aws.amazon.com/de_de/codecommit/latest/userguide/setting-up-ide-c9.html)

[Referenz-2 Erste Schritte mit Git und AWS CodeCommit](https://docs.aws.amazon.com/codecommit/latest/userguide/getting-started.html)
  
  
## Tmux Multiplexor  

    Ctrl + b --> Tastaturkürzel (Prefix)
    New Window --> Prefix + c
    Fenster Horizontaler Teilen --> Prefix + "
    Fenster Vertical Teilen --> Prefix + %  
    Session Liste --> Prefix + s
      
## SSH
So erstellen man Private und Öffentliche Schlüssel:

    ssh-keygen


Überprüfen ob id_rsa und id_rsa.pub existiert:

    cd
    ls -lah .ssh
    
Falls man sich mit anderem Server und/oder GitHub verbinden möchten, muss man folgenden Befehl kopieren und in den Authorized Keys Datei (~/.ssh/authorized_keys) einfügen:

    cat ~/.ssh/id_rsa.pub
  
  
## sichern und härten des OpenSSH-Servers  
[Tecmint](https://www.tecmint.com/secure-openssh-server)  
[Cyberciti](https://www.cyberciti.biz/tips/linux-unix-bsd-openssh-server-best-practices.html)  


## sich per ssh über PowerShell einloggen

[ssh-PowerShell](https://4sysops.com/archives/powershell-remoting-with-ssh-public-key-authentication/)

In EC2-Instance (Cloud9 >> Sicherheitgruppen >> neu Regeln für eingehende Daten hinzufügen >> SSH from anywhere)

  

## ssh Key erstellen

    cd
    ssh-keygen 
Eingabe Taste x3

    ls -lah .ssh
   
  

## Einrichtung für HTTPS-Benutzer mit Git-Anmeldeinformationen

Detailliertere Anweisungen findet man in der Dokumentation: 
[mehr Anzeigen](https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-gc.html?icmpid=docs_acc_console_connect_np)

# Erstellen einer EC2-Instanz mit NGINX + AWS-CLI (23.052023)
1. Erstellen Sie eine t.2micro EC2-Instanz mit Ubuntu 22.04 als Betriebssystem
2. Erstellen Sie ein Skript, das alle Pakete aktualisiert, NGINX installiert und den Dienst startet
3. Stellen Sie sicher, dass NGINX auf der Instanz ausgeführt wird
4. Beenden Sie die EC2-Instanz
  

## Schlüsselpaar erstellen 

    aws ec2 create-key-pair --key-name NGINX-Paula --query 'KeyMaterial' --output text > NGINX-Paula.pem  
    
PowerShell

    aws ec2 create-key-pair --key-name MyTestKey --query 'KeyMaterial' --output text | out-file -encoding ascii -filepath MyTestKey.pem

Überprüfe, ob er generiert wurde

    aws ec2 describe-key-pairs  --key-name Nginx-Paula
  

## Erstellen einer Sicherheitsgruppe

Detailliertere Anweisungen finden Sie in der Dokumentation.
[mehr Anzeigen](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/create-security-group.html)

    aws ec2 create-security-group --group-name paux-sg --description "Sicherheitsgruppe fuer EC2Nginx"

Für HTTP:

    aws ec2 authorize-security-group-ingress --group-id sg-016907f268702a19b --protocol tcp --port 80 --cidr 0.0.0.0/0

Für SSH:

    aws ec2 authorize-security-group-ingress --group-id sg-016907f268702a19b --protocol tcp --port 22 --cidr 0.0.0.0/0 

Details zur Sicherheitsgruppe anzeigen: 

    aws ec2 describe-security-groups --group-ids sg-016907f268702a19b
  

## Erstellen einer EC2-Instanz

1. AWS Konsole > EC2 > AMI Katalog > Suchen Ubuntu 22.04 (ami-0ec7f9846da6b0f61)
2. nginxscript.sh Datei erstellen und Script schreiben für Installation und Starten von Nginx
3. Erstellen EC2-Instance mit Ubuntu + Nginx
  

### Bash-Script für NGINX 

    touch nginxscript.sh
    
Skript im Texteditor vim und speichern Sie dann :wq
Skript zum installieren und starten von NGINX:

    #!/bin/bash 
    apt update -y 
    apt upgrade -y 
    apt-get install -y nginx 
    systemctl start nginx 
    systemctl enable nginx
  
### EC2 Instanz starten

    aws ec2 run-instances --image-id ami-0ec7f9846da6b0f61 --count 1 --instance-type t2.micro --key-name Nginx-Paula --security-groups paux-sg --user-data file://nginxscript.sh
  
### SSH Verbindungen mit EC2Nginx

Berechtigungen vom Schlüsselpaar ändern  
  

    chmod 400 Nginx-Paula.pem
    ssh -i Nginx-Paula.pem ubuntu@18.196.193.125  
    
### Verschieben Paket Manager unter Linux Distributionen

1. Debian basierte >> apt-get, apt, Ubuntu, Linuxmint, Kali Linux
2. Archlinux basierte >> pacman, yay >> Archlinux, Manjaro, Garuda Linux...
3. Fedora >> dnf
4. CentOS = Amazon Linux >> yum
5. OpenSuse >> zypper 
6. Alpine Linux >> apk  


* pip (Python)
* npm (NodeJS)
* brew (MacOS)
* cargo (Rust)  
  

### Verschiedene Shells

* sh (Shell)
* bash (Bourn again Shell)
* ksh (Korn Shell)
* csh (C-Shell)
* zsh
* fish-shell  
---  

  
[APT Cheat Sheet](https://opensource.com/sites/default/files/gated-content/osdc_cheatsheet-apt-2021.5.30.pdf)  

# Einrichten einer Webseite unter Nginx und Linux (Ubuntu & Debian) (24.05.2023)  

Detailliertere Anweisungen findet man in der Dokumentation: 
[mehr Anzeigen](http://flsilva.com/blog/how-to-configure-a-website-on-nginx-and-linux-tutorial/)  


## Forward-Proxy vs. Reverse-Proxy

Der Hauptunterschied zwischen einem Reverse-Proxy und einem Forward-Proxy besteht darin, dass ein Forward-Proxy isolierten Computern in einem privaten Netzwerk die Verbindung mit dem öffentlichen Internet ermöglicht, während ein Reverse-Proxy Computern im Internet den Zugriff auf ein privates Subnetz ermöglicht.  
[Schaubild](http://www.celinio.net/techblog/wp-content/uploads/2011/09/ReverseProxy.jpg)  
[mehr Anzeigen](https://www.theserverside.com/feature/Forward-proxy-vs-reverse-proxy-Whats-the-difference)  

## Zusammenfassung eine VPC, ein öffentliches Subnetz, ein privates Subnetz, ein NAT-Gateway, eine Routing-Tabelle, eine Sicherheitsgruppe und eine EC2-Instanz erstellen  
[mehr Anzeigen](https://dev.to/mkabumattar/how-to-create-an-aws-ec2-instance-using-aws-cli-32ek)   
  
## Partitionierung 
/EFI = 300 Mb -> Format: FAT32/vFAT
/boot = bis 512 Mb -> ext2/3/4
/swap = 1 Gb RAM -> 2 Gb
        2 Gb RAM -> 4 Gb
        4 Gb RAM -> 2 Gb
        12 Gb RAM -> kein swap
/   = ab 50 Gb
/home = was ihr möchtet / den Rest der Partition  
  
  
[Was ist eine AWS Availability Zone?](https://www.techtarget.com/searchcloudcomputing/tip/Understand-AWS-Regions-vs-Availability-Zones)    
  
  
## Was ist eine AWS-Region?

Eine AWS-Region ist ein Cluster von Rechenzentren in einem bestimmten geografischen Gebiet, beispielsweise im Nordosten der USA oder in Westeuropa. Es empfiehlt sich, eine Region auszuwählen, die geografisch in der Nähe der Benutzer liegt. Dies reduziert die Latenz, da die Daten schneller beim Benutzer ankommen.

Jede AWS-Region umfasst mehrere AZs. Allerdings ist jede AZ auf eine bestimmte AWS-Region beschränkt. Sie können mehrere AZs innerhalb einer Region verwenden, Sie können jedoch nicht dieselbe AZ in mehreren Regionen verwenden.  
  
---  

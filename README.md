# Ops-generiche

## 1) Installati Virtualbox, GIT, Vagrant sul tuo Mac Book

apt-get update; apt-get install -y virtualbox git vagrant

## 2) Tramite Virtualbox installa una virtual machine Centos7 

vim Vagrantfile --> 

```
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.network :forwarded_port, guest: 80, host: 4567
end
```

```
cd ~/Vagrant

vagrant up

vagrant ssh default
```

```
[vagrant@localhost ~]$ cat /etc/redhat-release 

CentOS Linux release 7.8.2003 (Core)
```

## 3) Crea un file con touch e inserisci delle stringhe con ">>" 

```
[vagrant@localhost ~]$ echo "The following providers are enabled: virtualbox, libvirt, hyperv and vmware_desktop (the latter should work for vmware_fusion, vmware_workstation and vmware_desktop)." >> prova.txt
```

## 4) Leggi un file con 'cat', Greppa una stringa a tuo piacere con 'grep'

```
[vagrant@localhost ~]$ grep -i "virtual" prova.txt 
The following providers are enabled: virtualbox, libvirt, hyperv and vmware_desktop (the latter should work for vmware_fusion, vmware_workstation and vmware_desktop).
```

**OPPURE** -->

```
[vagrant@localhost ~]$ cat prova.txt | grep -i "virtual"
The following providers are enabled: virtualbox, libvirt, hyperv and vmware_desktop (the latter should work for vmware_fusion, vmware_workstation and vmware_desktop).
```

## 5) Leggi file con vim, less e more 

less prova.txt oppure more prova.txt

## 6) Raggiungi la VM installata tramite un indirizzo IP locale

dentro virtualbox -> settings sulla macchina virtuale creata -> network -> adapter2 -> wlo1

su nuova console:

```
ale@mint ~/Vagrant $ ssh vagrant@192.168.1.215
The authenticity of host '192.168.1.215 (192.168.1.215)' can't be established.
ECDSA key fingerprint is SHA256:Xq6hI79G5AN3CSHaw7RcM8d13UAL3hG6ifCerKWYDKw.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.1.215' (ECDSA) to the list of known hosts.
vagrant@192.168.1.215's password: 
Last login: Mon Mar  8 11:02:59 2021 from 10.0.2.2
[vagrant@localhost ~]$ 
```

## 7) Disabilita Selinux 

**per disabilitare selinux**:

cd /etc/selinux

ho editato il file config

SELINUX=disabled

ho fatto il reboot della macchina -->

```
[vagrant@localhost ~]$ getenforce
Disabled

[vagrant@localhost ~]$ sestatus
SELinux status:                 disabled
```

## 8) Installa un Apache Webserver (httpd) ed esponi una pagina che dica "Hello World". Alla fine dell'esercizio dal tuo browser potrai fare http://<ip_della_tua_macchina> e visualizzare il sito web.

```
sudo yum install -y httpd

sudo systemctl start httpd

sudo systemctl enable httpd
```

```
[vagrant@localhost ~]$ systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2021-03-08 11:21:33 UTC; 25s ago
     Docs: man:httpd(8)
           man:apachectl(8)
 Main PID: 1012 (httpd)
   Status: "Total requests: 0; Current requests/sec: 0; Current traffic:   0 B/sec"
   CGroup: /system.slice/httpd.service
           ├─1012 /usr/sbin/httpd -DFOREGROUND
           ├─1013 /usr/sbin/httpd -DFOREGROUND
           ├─1014 /usr/sbin/httpd -DFOREGROUND
           ├─1015 /usr/sbin/httpd -DFOREGROUND
           ├─1016 /usr/sbin/httpd -DFOREGROUND
           └─1017 /usr/sbin/httpd -DFOREGROUND
```

```
[root@localhost html]# history
    1  pwd
    2  cd /var/www/
    3  ls
    4  cd html/
    5  echo "hello world" >> index.html
    6  ls
    7  cat index.html 
    8  systemctl restart
    9  systemctl restart httpd
```

```
[vagrant@localhost html]$ curl http://192.168.1.215
hello world
```
## Copia un file sulla macchina tramite scp rsync

```
ale@mint ~/Desktop $ dd if=/dev/zero of=/home/ale/Desktop/file bs=1024 count=100000
100000+0 records in
100000+0 records out
102400000 bytes (102 MB, 98 MiB) copied, 0,229363 s, 446 MB/s
```

```
ale@mint ~/Desktop $ ls
total 192M
-rw-rw-r--  1 ale ale    0 Dec  9  2019 ...
-rw-rw-r--  1 ale ale  98M Mar 10 10:15 file
```

```
ale@mint ~/Desktop $ scp file vagrant@192.168.1.215:/home/vagrant
vagrant@192.168.1.215's password: 
file                                                                                                                                  100%   98MB  97.7MB/s   00:01   
```
## Crea sul macchina un archivio con tar e gzip

```
[vagrant@localhost ~]$ tar cvfz file.tgz file
file
```

```
[vagrant@localhost ~]$ ls
file  file.tgz  prova.txt
```


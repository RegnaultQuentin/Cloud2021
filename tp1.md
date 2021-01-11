# TP1 Prise en main de Docker
## I. Prise en main

* Lister les conteneurs actifs, et mettre en évidence le conteneur lancé sur le sleep

```
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
48685e318d9b        alpine              "sleep 9999"        About a minute ago   Up About a minute                       vigilant_kilby
```
Son est vigilant_kilby et son id 48685e318d9b

* Mise en évidence d'une partie de l'isolation mise en place par le conteneur. Montrer que le conteneur utilise :

    - une arborescence de processus différente :
    ```
    / # pstree
    sleep
    ```

    - des cartes réseau différentes :
    ```
    / # ifconfig
    eth0      Link encap:Ethernet  HWaddr 02:42:AC:11:00:02
            inet addr:172.17.0.2  Bcast:172.17.255.255  Mask:255.255.0.0
            UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
            RX packets:70 errors:0 dropped:0 overruns:0 frame:0
            TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
            collisions:0 txqueuelen:0 
            RX bytes:9173 (8.9 KiB)  TX bytes:0 (0.0 B)

    lo        Link encap:Local Loopback
            inet addr:127.0.0.1  Mask:255.0.0.0
            UP LOOPBACK RUNNING  MTU:65536  Metric:1
            RX packets:0 errors:0 dropped:0 overruns:0 frame:0
            TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
            collisions:0 txqueuelen:1000 
            RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
    ```
    - des utilisateurs systèmes différents :
    ```
    BusyBox v1.31.1 () multi-call binary.
    root:x:0:0:root:/root:/bin/ash
    bin:x:1:1:bin:/bin:/sbin/nologin
    daemon:x:2:2:daemon:/sbin:/sbin/nologin
    adm:x:3:4:adm:/var/adm:/sbin/nologin
    lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
    sync:x:5:0:sync:/sbin:/bin/sync
    shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
    halt:x:7:0:halt:/sbin:/sbin/halt
    mail:x:8:12:mail:/var/mail:/sbin/nologin
    news:x:9:13:news:/usr/lib/news:/sbin/nologin
    uucp:x:10:14:uucp:/var/spool/uucppublic:/sbin/nologin
    operator:x:11:0:operator:/root:/sbin/nologin
    man:x:13:15:man:/usr/man:/sbin/nologin
    postmaster:x:14:12:postmaster:/var/mail:/sbin/nologin
    cron:x:16:16:cron:/var/spool/cron:/sbin/nologin
    ftp:x:21:21::/var/lib/ftp:/sbin/nologin
    sshd:x:22:22:sshd:/dev/null:/sbin/nologin
    at:x:25:25:at:/var/spool/cron/atjobs:/sbin/nologin
    squid:x:31:31:Squid:/var/cache/squid:/sbin/nologin
    xfs:x:33:33:X Font Server:/etc/X11/fs:/sbin/nologin
    games:x:35:35:games:/usr/games:/sbin/nologin
    cyrus:x:85:12::/usr/cyrus:/sbin/nologin
    vpopmail:x:89:89::/var/vpopmail:/sbin/nologin
    ntp:x:123:123:NTP:/var/empty:/sbin/nologin
    smmsp:x:209:209:smmsp:/var/spool/mqueue:/sbin/nologin
    guest:x:405:100:guest:/dev/null:/sbin/nologin
    nobody:x:65534:65534:nobody:/:/sbin/nologin 
    ```
    - des points de montage (les partitions montées) différents :
    ```
    / # cat /etc/fstab
    /dev/cdrom    /media/cdrom    iso9660    noauto,ro 0 0
    /dev/usbdisk    /media/usb    vfat    noauto,ro 0 0
    ```
*  Détruire le conteneur avec docker rm
    ```
    quentin@quentin-GL703VD:~$ docker rm vigilant_kilby
    vigilant_kilby
    ```
* Conteneur NGINX 

    - Lancer le conteneur en démon et utiliser -p pour partager un port de l'hôte vers le port 80 du conteneur :
    ```
    docker run --name nginx -d -p 80:80 nginx   
    ```
    - Visiter le service web en utilisant l'IP de l'hôte
    <img src="https://cdn.discordapp.com/attachments/582825013690892290/798214450858557450/unknown.png">

## 2. Gestion d'images
* Récupérer une image de Apache en version 2.2 :
```
quentin@quentin-GL703VD:~$ docker pull httpd:2.2
2.2: Pulling from library/httpd
f49cf87b52c1: Pull complete 
24b1e09cbcb7: Pull complete 
8a4e0d64e915: Pull complete 
bcbe0eb4ca51: Pull complete 
16e370c15d38: Pull complete 
Digest: sha256:9784d70c8ea466fabd52b0bc8cde84980324f9612380d22fbad2151df9a430eb
Status: Downloaded newer image for httpd:2.2
docker.io/library/httpd:2.2
```
* Une fois lancé : 
```
CONTAINER ID        IMAGE               COMMAND              CREATED             STATUS              PORTS                NAMES
146bb45a1dcb        httpd:2.2           "httpd-foreground"   9 seconds ago       Up 7 seconds        0.0.0.0:80->80/tcp   apache
```
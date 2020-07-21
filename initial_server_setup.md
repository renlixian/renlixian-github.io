`ssh root@serverip`

`apt update`

`apt install sudo`

`adduser jackren`

`usermod -aG sudo jackren`

`su - jackren`

sudo apt update

switch to local

`ssh-keygen`
`ssh-copy-id jackren@server.ip`

## INSTALL TRANSMISSION

`sudo apt install transmission-daemon`
`sudo systemctl stop transmission-daemon.service`
`sudo nano /etc/transmission-daemon/settings.json`

	"rpc-password": "mypassword"
	"rpc-whitelist-enabled": false

	sudo systemctl start transmission-daemon.service

http://serverip:9091


## INSTALL MLDONKEY


```
{
sudo apt install mldonkey-server
sudo systemctl stop mldonkey-server
sudo nano /var/lib/mldonkey/download.ini
allowed_ips = [0.0.0.0/0;]
((mlnet > /dev/null 2>&1 &)) optional??
sudo systemctl start mldonkey-server

useradd jackren password
}
```

## INSTALLING ARIA2

## INSTALL V2RAY

wget https://install.direct/go.sh
sudo bash go.sh 


note down  PORT & UUID


windows PC:
install v2ray windows terminal, configure with PORT & UUID
linux PC:
same as server version, copy the config.json file in the windows PC to linux PC /etc/v2ray/
restart

download pac file copy to your server and set network proxy to auto, http://serverip/xx.pac 


--------------

BATCH-RENAME FILES 
--------------

$ for i in *.html; do mv -- "$i" "${i%.html}.txt"; done

ffmpeg -i video.mp4 subtitle.srt
$ for i in *.mkv; do sudo mkvextract tracks "$i" 4:"${i%.mkv}.srt"; done

$ for i in {01..06}; do mv *$i* xx${i}.txt; done


$ find . -depth -name "*.html" -exec sh -c 'f="{}"; mv -- "$f" "${f%.html}.php"' \;

$ 
rename -f 's/.html/.php/' *.html
rename 'y/ /_/' * (replacing spaces in filenames with underscores)


$ for foo in *.m4a; do ffmpeg -i "$foo" -acodec libmp3lame -aq 2 "${foo%.m4a}.mp3"; done


-----------------

DOWNLOAD
------------

-----
wget --max-redirect=1 http://xxx.file.to.download
--trust-server-names
wget -c --limit-rate=500k -i xxx.txt
curl --range 0-199999999 -o myiso.part1 http://.../xx.iso
curl --range 200000000-399999999 -o myiso.part2 http://.../xx.iso
cat myiso.part* > my.iso

-------------
INSTALLING ARIA2
-------------
apt update
apt install aria2

**CONFIGURATION FILE**

default: ~/.aria2/aria2.conf (need to be created by user)
customed: /etc/aria2.conf (need to run: aria2c --conf-path=/etc/aria2.conf)

##files
dir=/user/downloads
file-allocation=falloc
continue=true
daemon=true
disk-cache=32M

##logging
log=/user/aria2/aria2.log (need to be created by user)
console-log-level=warn
log-level=notice

##downloads
max-concurrent-downloads=5
max-connection-per-server=5
min-split-size=20M
split=4
disable-ipv6=true

##sessions
force-save=true
input-file=/user/aria2/ses/aria2.session (need to be created by user)
save-session=/user/aria2/ses/aria2.session (need to be created by user)
save-session-interval=10

##security
http-auth-challenge=true
check-certificate=false
enable-rpc=true
rpc-listen-all=true
rpc-secret=jackren75 (or create a good one by running: openssl rand -hex 15) 

#rpc-secure=true	
#rpc-allow-origin-all=true
#rpc-certificate=/user/aria2/cet/aria2.pfx

##ports
rpc-listen-port=6800
http-proxy=127.0.0.1:4204

##others
summary-interval=120
enable-dht=true

##times
timeout=600
retry-wait=30
max-tries=50

**SYSTEMD SERVICE**

to start aria2 as a systemd service (daemon)

create file
sudo nano /lib/systemd/system/aria2.service

[Unit]
Description=Aria2c download manager
After=network.target
    
[Service]
Type=simple
User=jackren
RemainAfterExit=yes
ExecStart=/usr/bin/aria2c --conf-path=/user/aria2/conf/aria.conf
ExecReload=/usr/bin/kill -HUP $MAINPID
ExecStop=/usr/bin/kill -s STOP $MAINPID
RestartSec=1min
Restart=on-failure
    
[Install]
WantedBy=multi-user.target

sudo systemctl start/enable/status aria2


apt install nginx

sudo git clone https://github.com/ziahamza/webui-aria2.git

copy the "docs" folder to /var/www/html and rename it aria2, then access webui using browser by https://your_server_ip/aria2


-------------------
Just to add more flavor to this question, I'd also recommend that you take a look at this:

history -d $((HISTCMD-1)) && echo '[PASSWORD]' | sudo -S shutdown now

You could use this to shutdown your computer after your wget command with a ; perhaps or in a bash script file.

This would mean you don't have to stay awake at night and monitor until your download as (un)successfully run.
-------------------

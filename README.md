# OCSPd
This how-to aims to start Online Certificate Status Protocol responder as a daemon.
## Prerequisite
Before making anything, you must follow this [tutorial](https://jamielinux.com/docs/openssl-certificate-authority/index-full.html) to create the following items:
* Root Certificate Authority
* Intermediate Certificate Authority
* OCSP private key
* OCSP certificate signin request
* OCSP certificate

The operating system is Ubuntu server 18.04.4 LTS
## Installation
#### Download needed files
You need to download two files in your home directory:
```
cd ~
wget https://raw.githubusercontent.com/mikael-andre/OCSPd/master/ocspd.service
wget https://raw.githubusercontent.com/mikael-andre/OCSPd/master/ocspd
```
#### Copy files
Once downloaded, you have to copy init script in `/etc/systemd/system` and configuration file in `/etc/default`:
```
sudo cp -p ocspd.service /etc/systemd/system/ocspd.service
sudo cp -p ocspd /etc/default/ocspd
```
#### Give good rights
The init script needs excute rights:
```
chmod 664 /etc/systemd/system/ocspd.service 
```
#### Create log file
In order to log requests and responses, you have to create a new log file in `/var/log` folder by the following command:
```
touch /var/log/ocspd.log
```
#### Reload Systemd
After giving theses privileges, you have to reload systemd
```
systemctl daemon-reload
```
#### Automatic start
Finaly, if you want init script to start on boot, you have to type this command
```
systemctl enable ocspd
```
## Verifications
By the same command, `systemctl status ocspd`, you can verify if OCSPd is started or stopped.
#### Started
```
root@lnx001:~# systemctl status ocspd
● ocspd.service - Online Certificate Status Protocol daemon
   Loaded: loaded (/etc/systemd/system/ocspd.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2020-04-17 14:34:11 UTC; 1s ago
 Main PID: 3169 (openssl)
    Tasks: 1 (limit: 2353)
   CGroup: /system.slice/ocspd.service
           └─3169 /usr/bin/openssl ocsp -port 2560 -text -index /root/ca/intermediate/index.txt -CA /root/ca/intermediate/certs/cachaincert.pem -rkey /root/ca/intermediate/private/n

Apr 17 14:34:11 lnx001.ibreizh.lan systemd[1]: Started Online Certificate Status Protocol daemon.
Apr 17 14:34:11 lnx001.ibreizh.lan openssl[3169]: ocsp: waiting for OCSP client connections...
```
#### Stopped
```
root@lnx001:~# systemctl status ocspd
● ocspd.service - Online Certificate Status Protocol daemon
   Loaded: loaded (/etc/systemd/system/ocspd.service; enabled; vendor preset: enabled)
   Active: inactive (dead)

Apr 17 14:29:28 lnx001.ibreizh.lan systemd[1]: Stopping Online Certificate Status Protocol daemon...
Apr 17 14:29:28 lnx001.ibreizh.lan systemd[1]: Stopped Online Certificate Status Protocol daemon.
```

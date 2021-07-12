# Integrating Zimbra and Z-push on Centos 7
This repository is source for integrating Zimbra Single Server and Z-Push + Zimbra Backend to achieve ActiveSync on Zimbra Open Source Edition.

I’m using the following versions in this guide:
- Zimbra 8.8.15 GA Release Patch 23
- Z-Push 2.6.3
- Z-Push Zimbra Backend Release 70

Firstly, we need to install epel repo for installation the depencies.

> yum install epel-release -y
> 
> yum install git php-cli php-soap php-process php-mbstring -y

**then clone repo to server;**

> git clone https://github.com/sekozzi/zimbra_z-push.git
> 
> cd zimbra_z-push/


**then create folders for log;**

> mkdir /var/lib/z-push /var/log/z-push
> 
> chmod 755 /var/lib/z-push /var/log/z-push
> 
> chown zimbra:zimbra /var/lib/z-push /var/log/z-push

**then move z-push folder over /opt;**

> cp -rvf z-push /opt

Now you need to some changes. 

Firstly, i use Europe/Istanbul timezone. You can change **TIMEZONE** on **/opt/z-push/config.php**

> define('TIMEZONE', 'Europe/Istanbul');

and you need to change **ZIMBRA_URL** on **/opt/z-push/backend/zimbra/config.php**

> define('ZIMBRA_URL', 'https://mail.domain.com');

and finally, you need to change **TIMEZONE** and **ZPUSH_HOST** on **/opt/z-push/autodiscover/config.php**

> define('TIMEZONE', 'Europe/Istanbul');
> 
> define('ZPUSH_HOST', 'mail.domain.com');


**then create symbolic link;**

> ln -sf /opt/z-push /opt/zimbra/jetty/webapps/

**then copy php script to /usr/bin;**

> cp php-cgi-fix.sh /usr/bin/php-cgi-fix.sh
> 
> chmod +x /usr/bin/php-cgi-fix.sh

**then backup and replace jetty.xml.in;**

> cp /opt/zimbra/jetty/etc/jetty.xml.in /opt/zimbra/jetty/etc/jetty.xml.in.bck
> 
> cp jetty.xml.in /opt/zimbra/jetty/etc/jetty.xml.in
> 
> chown zimbra.zimbra /opt/zimbra/jetty/etc/jetty.xml.in

**then add zpush.ini into php.d folder;**

> cp zpush.ini /etc/php.d/zpush.ini

**finally restart zimbra mailbox;**

> su - zimbra -c 'zmmailboxdctl restart'

**Testing Exchange ActiveSync**

By visting your ActiveSync URL on your Zimbra server like:

https://mail.domain.com/Microsoft-Server-ActiveSync

If everything is fine, you would be prompted to login, login with one of your accounts. if you get to Z-Push page, like the following:

![enter image description here](https://www.linkpicture.com/q/z-push-ss.jpg)

It means that Z-Push is successfully installed.


**add DNS srv record for autodiscover;**

if you want to use activesync feature on outlook or mobile device, you need to add DNS srv record. Example is as following. You can add the srv record over your domain dns provider.

> _autodiscover._tcp.domain.com SRV 443 mail.domain.com








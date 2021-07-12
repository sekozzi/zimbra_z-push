# Integrating Zimbra and Z-push
This repository is source for integrating Zimbra Single Server and Z-Push + Zimbra Backend to achieve ActiveSync on Zimbra Open Source Edition.

# Integrating on CentOS 7
Firstly, we need to install epel repo for installation the depencies.

> yum install epel-release -y
> 
> yum install git php-cli php-soap php-process php-mbstring -y

then clone repo to server;

> git clone https://github.com/sekozzi/zimbra_z-push.git
> 
> cd zimbra_z-push/


then create folders for log;

> mkdir /var/lib/z-push /var/log/z-push
> 
> chmod 755 /var/lib/z-push /var/log/z-push
> 
> chown zimbra:zimbra /var/lib/z-push /var/log/z-push

then move z-push folder on /opt;

> cp -rvf z-push /opt/

then create symbolic link;

> ln -sf /opt/z-push /opt/zimbra/jetty/webapps/

then php script on /usr/bin;

> cp php-cgi-fix.sh /usr/bin/php-cgi-fix.sh
> 
> chmod +x /usr/bin/php-cgi-fix.sh

then backup and replace jetty.xml.in;

> cp /opt/zimbra/jetty/etc/jetty.xml.in /opt/zimbra/jetty/etc/jetty.xml.in.bck
> 
> cp jetty.xml.in /opt/zimbra/jetty/etc/jetty.xml.in
> 
> chown zimbra.zimbra /opt/zimbra/jetty/etc/jetty.xml.in

then add zpush.ini into php.d folder;

> cp zpush.ini /etc/php.d/zpush.ini

finally restart zimbra mailbox;

> su - zimbra -c 'zmmailboxdctl restart'

# add DNS srv record for autodiscover;
if you want to use activesync feature on outlook or mobile device, you need to add DNS srv record. Example is as following. You can add the srv record over your domain dns provider.

> _autodiscover._tcp.domain.com SRV 443 mail.domain.com








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

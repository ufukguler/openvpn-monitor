```shell
#!/bin/bash
echo "OpenVPN kurulumu baslatiliyor..."
wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh
apt-get -y install git apache2 libapache2-mod-wsgi python-geoip2 python-ipaddr python-humanize python-bottle python-semantic-version geoip-database-extra geoipupdate
echo "Apache config ayalari yapiliyor..."
echo "WSGIScriptAlias /openvpn-monitor /var/www/html/openvpn-monitor/openvpn-monitor.py" >> /etc/apache2/conf-available/openvpn-monitor.conf
echo "<Directory /var/www/html/openvpn-monitor>" >> /etc/apache2/conf-available/openvpn-monitor.conf
echo "Options FollowSymLinks" >> /etc/apache2/conf-available/openvpn-monitor.conf
echo "AllowOverride All" >> /etc/apache2/conf-available/openvpn-monitor.conf
echo "</Directory>" >> /etc/apache2/conf-available/openvpn-monitor.conf
a2enconf openvpn-monitor
systemctl restart apache2
echo "OpenVPN-Monitor kurulumu baslatiliyor..."
cd /var/www/html
git clone https://github.com/ufukguler/openvpn-monitor.git
echo "duplicate-cn" >> /etc/openvpn/server/server.conf
echo "management 127.0.0.1 5555" >> /etc/openvpn/server/server.conf
service openvpn restart
service openvpn-server@server restart
echo "AuthType Basic" >> /var/www/html/openvpn-monitor/.htaccess
echo 'AuthName "Restricted Files"' >> /var/www/html/openvpn-monitor/.htaccess
echo "AuthUserFile /var/www/.monitor" >> /var/www/html/openvpn-monitor/.htaccess
echo "Require valid-user" >> /var/www/html/openvpn-monitor/.htaccess
read -p 'monitor icin username: ' uservar
echo "$uservar kullanicisi olusturuluyor..."
echo "$uservar icin parola olusturun..."
sudo htpasswd -c /var/www/.monitor $uservar
systemctl restart apache2

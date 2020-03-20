```shell
wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh
apt-get -y install git apache2 libapache2-mod-wsgi python-geoip2 python-ipaddr python-humanize python-bottle python-semantic-version geoip-database-extra geoipupdate
echo "WSGIScriptAlias /openvpn-monitor /var/www/html/openvpn-monitor/openvpn-monitor.py" > /etc/apache2/conf-available/openvpn-monitor.conf
a2enconf openvpn-monitor
systemctl restart apache2
cd /var/www/html
git clone https://github.com/ufukguler/openvpn-monitor.git
echo "management 127.0.0.1 1194" >> /etc/openvpn/server/server.conf
service openvpn-server@server restart

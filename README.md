##### Debian / Ubuntu

```shell
wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh
apt-get -y install git apache2 libapache2-mod-wsgi python-geoip2 python-ipaddr python-humanize python-bottle python-semantic-version geoip-database-extra geoipupdate
echo "WSGIScriptAlias /openvpn-monitor /var/www/html/openvpn-monitor/openvpn-monitor.py" > /etc/apache2/conf-available/openvpn-monitor.conf
a2enconf openvpn-monitor
systemctl restart apache2
cd /var/www/html
git clone https://github.com/ufukguler/openvpn-monitor.git
echo "management 127.0.0.1 1194" >> /etc/openvpn/server/server.conf
```

##### CentOS / RHEL

```shell
yum install -y git httpd mod_wsgi python2-geoip2 python-ipaddr python-humanize python-bottle python-semantic_version geolite2-city GeoIP-update
echo "WSGIScriptAlias /openvpn-monitor /var/www/html/openvpn-monitor/openvpn-monitor.py" > /etc/httpd/conf.d/openvpn-monitor.conf
systemctl restart httpd

```

### Configure OpenVPN

Add the following line to your OpenVPN server configuration to run the
management console on 127.0.0.1 port 5555:

```
management 127.0.0.1 5555
```

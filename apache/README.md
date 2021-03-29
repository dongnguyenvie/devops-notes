```
<VirtualHost *:80>
    # This first-listed virtual host is also the default for *:80
    ServerName www.aws.quynhdev.com
    ServerAlias aws.quynhdev.com
    DocumentRoot "/var/www/quynhdev_http"
</VirtualHost>

<VirtualHost *:443>
    SSLEngine On
    ServerName www.aws.quynhdev.com
    ServerAlias aws.quynhdev.com
    SSLCertificateFile /etc/httpd/conf/https/certificate.crt
    SSLCertificateChainFile /etc/httpd/conf/https/ca_bundle.crt
    SSLCertificateKeyFile /etc/httpd/conf/https/server.key
    DocumentRoot "/var/www/html"
</VirtualHost>
```

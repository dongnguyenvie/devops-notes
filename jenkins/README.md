### Jenkins

```
https://github.com/jenkinsci/docker/blob/master/README.md
### Bad
openssl req -nodes -newkey rsa:2048 -keyout https.key -out https.csr -subj "/C=/ST=/L=/O=/OU=/CN=jenkin"
openssl x509 -req -sha256 -days 365 -in https.csr -signkey https.key -out https.crt

### Good and Work
# Generate certificate csr
openssl req -new > new.ssl.csr

# Create a key file for generating certificate
openssl rsa -in new.ssl.csr -out new.cert.key

# Create a csr file using the key file for 635 days
openssl x509 -in new.ssl.csr -out new.cert.cert -req -signkey new.cert.key -days 365

docker build -t nolan/jenkins -f dockerFile .
docker run -p 8083:8083 -v $(pwd)/jenkins_home:/var/jenkins_home nolan/jenkins
```

## Manually 
```
#This gist is related to blog post https://mohitgoyal.co/2017/02/08/securing-your-jenkins-environment-and-configure-for-auditing/

# Generate certificate csr
openssl req -new > new.ssl.csr

# Create a key file for generating certificate
openssl rsa -in new.ssl.csr -out new.cert.key

# Create a csr file using the key file for 635 days
openssl x509 -in new.ssl.csr -out new.cert.cert -req -signkey new.cert.key -days 365

# Creates intermediate pkcs12 file
openssl pkcs12 -export -out jenkins_keystore.p12 -passout 'pass:password' \
    -inkey new.cert.key -in new.cert.cert -name jenkinsci.com

# Create Java Keystore file
keytool -importkeystore -srckeystore jenkins_keystore.p12 \
    -srcstorepass 'password' -srcstoretype PKCS12 \
    -srcalias jenkinsci.com -deststoretype JKS \
    -destkeystore jenkins_keystore.jks -deststorepass 'password' \
    -destalias jenkinsci.com

# Move keystore file to Jenkins and assign Jenkins service account permissions for same
cd /var/lib/jenkins
mkdir keystore
cp ~/jenkins_keystore.jks /var/lib/jenkins/keystore/
chown -R jenkins.jenkins keystore/
chmod 700 keystore/

# Edit variables as mentioned in the blog post 

# Configure kernel to allow port forwarding
sysctl -w net.ipv4.ip_forward=1

#allow forwarding to localhost on eth0
sysctl -w net.ipv4.conf.eth0.route_localnet=1

# Configure iptables for port forwarding
# for remote connections
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 443 -j DNAT \
--to-destination 127.0.0.1:8443
iptables -A FORWARD -i eth0 -m state --state NEW -m tcp -p tcp \
-d 127.0.0.1 --dport 8443 -j ACCEPT

# for localhost connections
iptables -t nat -A OUTPUT -p tcp --dport 443 -d 127.0.0.1 \
-j DNAT --to-destination 127.0.0.1:8443

# Restart Jenkins service
systemctl stop jenkins.service
systemctl start jenkins.service
```
```
sudo su - www-user
ssh-keygen -t rsa
cd ~/.ssh

change id_rsa.pub to authorized_keys
mv id_rsa.pub authorized_keys
chmod 600 authorized_keys
```
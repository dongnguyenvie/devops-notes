### Create a user

```
user add [user_name]
passwd [user_name]

check information of user
id [user_name]

switch user
su - [user_name]
```

### Create a group

```
group add [group_name]
ex: group add accountant_gr

add user into a group
usermod -a -G [group_name] [user_name]
ex: user -a -G accountant_gr nolan

------

check permission
ls -la
```

### Permission

```
check permission
ls -la

dir:    d rwx rwx ---
file:   - rw- r-- r--
thứ tự phân quyền: user/group/other
d: dir
r: read per
w: write per
x: execute per
-: no per

permission table:
No.     Permission type         Symbol
0       No per                  ---
1       Execute                 --x
2       write                   -w-
3       Execute + write         -wx
4       Read                    r--
5       Read + write            rw-
6       Read + write + Execute  rwx (full)
chmod [permission] [file or folder name]
ex: chmod -Rf 777 abc (777 is user/group/other)


-----
cấp permission cho user and group
chown -Rf [user_name]:[group_name] [folder or file name]
ex1: chown -Rf root:accountant_gr accountant_dir
ex2: chown -Rf :accountant_gr accountant_dir (default user root)

```

### SSH by public key or pass

```
sudo su - www-user
ssh-keygen -t rsa
cd ~/.ssh

change id_rsa.pub to authorized_keys
mv id_rsa.pub authorized_keys
chmod 600 authorized_keys

### enable login by pwd:
/etc/ssh/sshd_config
-> PasswordAuthentication yes

if we want login by cert file
change ~/.ssh/public.key => authorized_keys
```

### Domain

```
check dns:
nslookup [domain]
```

### Some app
```
telnet localhost 80
```

### Vim
```
find: /[searching] (press n to next result)
:wq
i
```

### Tools

```
Poderosa: ssh
fugu: scp
```

## Создайте Ansible-роль, устанавливающую и запускающую FTP-сервер vsftpd.
Создайте директорию с названием вашей роли. Например, vsftpd-role.
Внутри директории роли создайте следующую структуру каталогов:
ansible-galaxy init vsftpd

vsftpd/
├── tasks/
│   └── main.yml
├── handlers/
│   └── main.yml
├── templates/
│   └── vsftpd.conf.j2
└── vars/
    └── main.yml

### В файле main.yml в директории tasks/ добавьте следующий код:
``` ansible
---
- name: Install vsftpd package
  apt:
    name: vsftpd
    state: present

- name: Copy vsftpd configuration file
  template:
    src: vsftpd.conf.j2
    dest: /etc/vsftpd.conf
    owner: root
    group: root
    mode: '0644'

- name: Start vsftpd service
  service:
    name: vsftpd
    state: started
    enabled: yes
``` 

### В файле main.yml в директории handlers/ добавьте следующий код:
```ansible
---
- name: Restart vsftpd service
  service:
    name: vsftpd
    state: restarted
```

### В файле vsftpd.conf.j2 в директории templates/ добавьте следующий код:
```ansible
listen=NO
listen_ipv6=YES
anonymous_enable=YES
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES
chroot_local_user=YES
secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
ssl_enable=NO
```
### В файле main.yml в директории vars/ добавьте следующий код:
```ansible
---
```
### Переменные для настройки vsftpd 

```ansible
---
- name: Установка и запуск FTP-сервера vsftpd
  hosts: ftpserver
  become: true
  roles:
    - vsftpd
```

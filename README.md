# test-oc3

## serveur proxy :
- Installation et configuration de Haproxy
- Prise en charge du FQDN test4.oc3n.net

## serveur web :
- Installation et configuration de Apache2
- Installation et configuration de PHP, PHP-FPM et pool FPM
- Installation d’un wordpress

## serveur sql :
- Installation et configuration de mariadb

# Utilisation

Remplir les variables d'inventaires pour correspondre aux cibles

Fichier inventories/cible/hosts
```
[all:vars]
ansible_connection=ssh
ansible_user=user
#ansible_ssh_pass=supersecret

[sql]
sqllab ansible_host=a.b.c.d

[web]
weblab ansible_host=a.b.c.d

[proxy]
proxylab ansible_host=a.b.c.d
```

Fichier inventories/cible/group_vars/all.yml
```
db_host: a.b.c.d
db_name: wordpress
db_user: user
db_user_password: supersecret
path_to_mariadb_conf: /etc/mysql/mariadb.conf.d/50-server.cnf
wordpress_url: https://wordpress.org/wordpress-6.4.3.tar.gz
wordpress_md5: "md5: 8e664626c12cb6daea37c8a90d8080d8"
wordpress_path: /tmp
wordpress_file_path: /wordpress/wp-config.php
wordpress_file_default: /wordpress/wp-config-sample.php
web_owner: www-data
web_group: www-data
web_path: /var/www/html
php_fpm_socket_path: /run/php/php-fpm.sock
site_domain: domain.example
apache_host: a.b.c.d
```

Générer une clef ssh si nécéssaire 
```
ssh-keygen
```
Déposer les clefs
```
ansible-playbook -i inventories/cible 00-upload-keys.yml -k
```
Pour éviter de retaper la passphrase
```
ssh-agent bash
ssh-add ~/.ssh/id_rsa
```

Déploiement sgbd
```
ansible-playbook -i inventories/labo 01-mariadb.yml -K
```

Déploiement Wordpress
```
ansible-playbook -i inventories/labo 02-webservers.yml -K
```

Déploiement haproxy
```
ansible-playbook -i inventories/labo 03-haproxy.yml -K
```

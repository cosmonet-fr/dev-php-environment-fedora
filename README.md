# Créer un environnement de développement sous Fedora

basé sur : https://doc.fedora-fr.org/wiki/Installation_et_configuration_d%27Apache

## Installation et configuration d'Apache (httpd)

### Installation d'apache
Apache est dans les dépots de la distribution:
```bash
# dnf install httpd-manual
```
### Activer les services

Démarrer:
```bash
# systemctl start httpd.service
```
Pour lancer Apache au démarrage du PC:
```bash
# systemctl enable httpd.service
```

### Utiliser le dossier home de chaques utilisateur du system
```bash
$ mkdir ~/public_html
```

```bash
# nano /etc/httpd/conf.d/messites.conf
```

```xml
# Hôte virtuel par défaut
<VirtualHost *:80>

         # Activation des sites de quelques utilisateurs : http://localhost/~nomutilisateur
         UserDir  enabled  nomutilisateur  unautre  undernier
         UserDir  public_html
        <Directory /home/*/public_html>
                AllowOverride all
                Options MultiViews Indexes SymLinksIfOwnerMatch IncludesNoExec
        </Directory>


         # Activation de l'adresse  : http://localhost/server-status
        <Location /server-status>
                SetHandler server-status
                Require ip 127.0.0.1
                Require ip ::1
        </Location>

         # Activation de l'adresse  : http://localhost/server-info
        <Location /server-info>
                SetHandler server-info
                Require ip 127.0.0.1
                Require ip ::1
        </Location>

</VirtualHost>
```

### Désactiver SELinux

```bash
# nano /etc/selinux/config
```

remplacer SELINUX=enforcing par SELINUX=disabled

----
## Installation et configuration de MariaDB

```bash
# dnf install mariadb-server
```

Créer un utilisateur et lui donner tout les droits


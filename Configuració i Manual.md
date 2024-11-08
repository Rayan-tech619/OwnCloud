<h1 style="color:red;">Instalació de OwnCloud By Rayan</h1>  
### Instal·lar la versió 7.4 de PHP a Ubuntu 24.04

Per a poder instal·lar ownCloud necessitarem la versió 7.4 de PHP al nostre sistema i fer les següents comandes:

Actualitza les llistes de paquets al vostre sistema. 

Instal·leu els requisits previs de PPA:
```bash
sudo apt install software-properties-common -y
```

Instal·la les eines necessàries per treballar amb els arxius de paquets personals (PPA).
```bash
LC_ALL=C.UTF-8 sudo add-apt-repository ppa:ondrej/php -y
```

Actualitza ara els repositoris:
```bash
sudo apt update
```

Instal·la les llibreries de PHP de la versió 7.4
```bash
sudo apt install php7.4 -y
```
```bash
sudo apt install -y php libapache2-mod-php7.4
```

```bash
sudo apt install -y php7.4-fpm php7.4-common php7.4-mbstring php7.4-xmlrpc php7.4-soap php7.4-gd php7.4-xml php7.4-intl php7.4-mysql php7.4-cli php7.4-ldap php7.4-zip php7.4-curl
```

Seleccioneu la versió de PHP que voleu:
```bash
sudo update-alternatives --config php
```

Activa els mòduls d'apache2 necessaris:
```bash
sudo a2enmod proxy_fcgi setenvif
```

```bash
sudo a2enconf php7.4-fpm
```

Reinicieu l'apache2:
```bash
sudo service apache2 restart
```
_____________________________________________________________________________________________________________________________________________________________________________________________________________
# Instal·lació i configuració d'aplicacions web

Per instal·lar una aplicació web hem de baixar el seu codi font i portar-lo al directori arrel del nostre servidor d'aplicacions, en el nostre cas, `apache2`. Quan instal·lem `apache2` es crea una carpeta a `/var/www/html` on, per defecte, el servidor web utilitza com a directori arrel.

## Instal·lació d'apache2, mysql i algunes llibreries al contenidor

1. Actualització de la màquina.
```bash
sudo apt update
```
```bash
sudo apt upgrade
```

2. Instal·lació del servidor web `apache2`.
```bash
sudo apt install -y apache2
```

3. Instal·lació del servidor de bases de dades `mysql-server`.
```bash
sudo apt install -y mysql-server
```

4. Instal·lació d'algunes llibreries de `php`, el llenguatge principal que utilitzen les aplicacions.
```bash
sudo apt install -y php libapache2-mod-php
```
```bash
sudo apt install -y php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-curl
```

5. Reiniciem el servidor apache2
```bash
sudo systemctl restart apache2
```

## Configuració de MySQL
### Accedim a la consola de MySQL
Des d'un terminal on siguem `root` hem d'executar la següent comanda:
```bash
sudo mysql
```

### Creació de la base de dades:
Un cop dins la consola de MySQL executem les comandes per a crear la base de dades. En aquest cas estem creant una base de dades amb el nom `bbdd`.

```bash
CREATE DATABASE bbdd;
```

### Creació d'un usuari
Tingueu en compte que s'haurà d'identificar la IP des de la qual s'accedirà a la base de dades, en aquest cas, `localhost`.

```bash
CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

### Donem privilegis a l'usuari:
```bash
GRANT ALL ON bbdd.* to 'usuario'@'localhost';
```

### Sortim de la base de dades
```bash
Exit
```

### Probem la connexió a la base de dades
Des d'un terminal amb un usuari sense privilegis hem de ser capaços de connectar introduïnt la nostra contrassenya.

```bash
mysql -u usuario -p
```

## Extra: permetre la connexió des d'una màquina remota
Per seguretat, MySQL no permet per defecte connexions que no siguin des de localhost. Si volem canviar aquest comportament hem de crear un altre usuari que accedirà des d'una màquina remota i estarà identificat pel nom d'usuari i la seva IP. Així doncs, poden existir diferents usuaris anomenats `usuario` que connecten des de diferents màquines.

### Canviem l'accés per defecte a la nostra màquina
Permetem l'accés des de qualsevol equip a la nostra base de dades. Editem l'arxiu `/etc/mysql/mysql.conf.d/mysqld.cnf`

```bash
vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

Busquem la línia següent:
```bash
bind-address = 127.0.0.1
```

Hem de canviar el `bind-address` per `0.0.0.0` i la línia ha de quedar així:
```bash
bind-address = 0.0.0.0
```

### Reiniciem el servidor
```bash
systemctl restart mysql
```

### Creació d'un usuari per a accedir des d'una màquina remota
Per accedir des d'una màquina remota, hauriem de crear un usuari nou identificat pel nom d'usuari i la IP de la màquina des de la qual accedirà.

```bash
CREATE USER 'usuario'@'192.168.22.100' IDENTIFIED WITH mysql_native_password BY 'password';
```

Hem de donar privilegis a l'usuari que accedirà des de la màquina remota.
Per accedir des de fora, hauriem de donar-li també privilegis a l'usuari a l'altra màquina:

```bash
GRANT ALL ON bbdd.* to 'usuario'@'192.168.22.100';
```

```bash
Exit
```
_____________________________________________________________________________________________________________________________________________________________________________________________________________
## Descarreguem els fitxers de l'aplicació web
Anem al directori `/var/www/html` i descomprimim allà els fitxers de l'aplicació web, heu de substituir `app-web.zip` per el nom del vostre fitxer que heu descarregat amb l'aplicació web i el nom de la carpeta `app-web` per la carpeta que us ha creat, si la vostra instal·lació de linux està en un idioma diferent al català, no tindreu la carpeta `Baixades`, modifiqueu la comanda per adaptarla a les vostrs necessitats.

```bash
sudo cp ~/Baixades/app-web.zip /var/www/html
```
Aneu al directori `/var/www/html`
```bash
cd /var/www/html
```
Descomprimiu el fitxer que heu baixat
```bash
sudo unzip app-web.zip
```
Copieu els fitxers a la carpeta `/var/www/html`, modifiqueu `app-web` pel nom del directori on s'ha descomprimit el vostre arxiu.
```bash
sudo cp -R app-web/. /var/www/html
```
Eliminem la carpeta creada quan hem fet l'`unzip`
```bash
sudo rm -rf app-web/
```

## Eliminem el fitxer `index.html` de l'`apache2`
```bash
sudo rm -rf /var/www/html/index.html
```
_____________________________________________________________________________________________________________________________________________________________________________________________________________
## Aplicació de permisos a les nostres aplicacions web
Un cop descomprimits els fitxers de l'aplicació web al directori `/var/www/html`, apliquem els següents permisos al directori `/var/www/html`

```bash
cd /var/www/html
```
```bash
sudo chmod -R 775 .
```
```bash
sudo chown -R usuario:www-data .
```
## Accedim al navegador per veure que tot funciona
Poseu la direcció http://localhost al navegador web i configureu la cloud.

Ara tindras que posar això en el configuració


* **usuari:** usuario
* **contrasenya:** password
* **base de dades:** bbdd
* **domini:** localhost

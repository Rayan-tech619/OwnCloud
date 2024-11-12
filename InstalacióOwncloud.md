# Instalació de OwnCloud By Rayan
### Instal·lar la versió 7.4 de PHP a Ubuntu 24.04

Per a poder instal·lar ownCloud necessitarem la versió 7.4 de PHP al nostre sistema:

https://download.owncloud.com/server/stable/owncloud-complete-20240724.zip
![image](https://github.com/user-attachments/assets/c952ab0b-3067-440d-bf59-6d3b164e615d)


Ara instal·leu els requisits previs de PPA:
```bash
sudo apt install software-properties-common -y
```
![image](https://github.com/user-attachments/assets/aed15e58-23a8-4553-9302-eecac3a939d8)


Instal·la les eines necessàries per treballar amb els arxius de paquets personals (PPA).
```bash
LC_ALL=C.UTF-8 sudo add-apt-repository ppa:ondrej/php -y
```
![image](https://github.com/user-attachments/assets/e3e96c06-cdb3-4737-9368-c19d43c9f5b8)

Actualitza ara els repositoris:
```bash
sudo apt update
```
![image](https://github.com/user-attachments/assets/3309b450-6d4c-448a-819b-f691c3e4feb3)

Instal·la les llibreries de PHP de la versió 7.4
```bash
sudo apt install php7.4 -y
```
![image](https://github.com/user-attachments/assets/f8bdcad8-b39e-43ef-ac8c-f52c49aa0e95)

```bash
sudo apt install -y php libapache2-mod-php7.4
```
![image](https://github.com/user-attachments/assets/c74b9be8-068f-43a4-9634-6df36e523057)

```bash
sudo apt install -y php7.4-fpm php7.4-common php7.4-mbstring php7.4-xmlrpc php7.4-soap php7.4-gd php7.4-xml php7.4-intl php7.4-mysql php7.4-cli php7.4-ldap php7.4-zip php7.4-curl
```
![image](https://github.com/user-attachments/assets/8a92f4c7-9274-4b06-a083-51e3ecd10432)

Seleccioneu la versió de PHP que voleu:
```bash
sudo update-alternatives --config php
```
![image](https://github.com/user-attachments/assets/7626ed8a-d88c-457d-bdc6-a955bafa92ad)

Activa els mòduls d'apache2 necessaris:
```bash
sudo a2enmod proxy_fcgi setenvif
```
![image](https://github.com/user-attachments/assets/4e975c86-4777-49bc-a3e5-2b2d78b8cdb8)

```bash
sudo a2enconf php7.4-fpm
```
![image](https://github.com/user-attachments/assets/9797b3ef-d1d4-4d9d-95a1-9bed0f9e924c)

Reinicieu l'apache2:
```bash
sudo service apache2 restart
```
![image](https://github.com/user-attachments/assets/010d4861-e59a-4c12-8c7c-232412d31008)

_____________________________________________________________________________________________________________________________________________________________________________________________________________
# Instal·lació i configuració d'aplicacions web

Per instal·lar una aplicació web hem de baixar el seu codi font i portar-lo al directori arrel del nostre servidor d'aplicacions, en el nostre cas, `apache2`. Quan instal·lem `apache2` es crea una carpeta a `/var/www/html` on, per defecte, el servidor web utilitza com a directori arrel.

## Instal·lació d'apache2, mysql i algunes llibreries al contenidor

Actualització de la màquina.
```bash
sudo apt update
```
![image](https://github.com/user-attachments/assets/3b003825-b9ff-4ac1-be1c-d9ecbd886799)

```bash
sudo apt upgrade
```
![image](https://github.com/user-attachments/assets/1cac3e19-f860-4de5-93b4-e1e0fb531b3f)

Instal·lació del servidor web `apache2`.
```bash
sudo apt install -y apache2
```
![image](https://github.com/user-attachments/assets/189fce09-0a1a-4664-b690-d168ad34ee06)

Instal·lació del servidor de bases de dades `mysql-server`.
```bash
sudo apt install -y mysql-server
```
![image](https://github.com/user-attachments/assets/f346f49b-d780-4f17-918a-b435a34831fa)

Instal·lació d'algunes llibreries de `php`, el llenguatge principal que utilitzen les aplicacions.
```bash
sudo apt install -y php libapache2-mod-php
```
![image](https://github.com/user-attachments/assets/0e708d47-6f10-4a3e-8d96-d6408a0be37a)

```bash
sudo apt install -y php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-curl
```
![image](https://github.com/user-attachments/assets/b0e1b12c-d7a7-4fbf-9d12-3ad48884ec98)

Reiniciem el servidor apache2
```bash
sudo systemctl restart apache2
```
![image](https://github.com/user-attachments/assets/a355c830-9844-4e9d-bf6e-a6a22bdf6f59)

## Configuració de MySQL
### Accedim a la consola de MySQL
Des d'un terminal on siguem `root` hem d'executar la següent comanda:
```bash
sudo mysql
```
![image](https://github.com/user-attachments/assets/39a9b7db-fb92-4c08-b2de-b0c511f3961e)

### Creació de la base de dades:
Un cop dins la consola de MySQL executem les comandes per a crear la base de dades. En aquest cas estem creant una base de dades amb el nom `bbdd`.

```bash
CREATE DATABASE bbdd;
```
![image](https://github.com/user-attachments/assets/7a615cc5-51f6-41a9-8883-ee0216347aa7)

### Creació d'un usuari
Tingueu en compte que s'haurà d'identificar la IP des de la qual s'accedirà a la base de dades, en aquest cas, `localhost`.

```bash
CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
![image](https://github.com/user-attachments/assets/0a3ae09c-7005-4f70-b7ed-657d12818742)

### Donem privilegis a l'usuari:
```bash
GRANT ALL ON bbdd.* to 'usuario'@'localhost';
```
![image](https://github.com/user-attachments/assets/5ca275c7-ecec-4059-81e0-ed85bad4bf47)

### Sortim de la base de dades
```bash
Exit
```
![image](https://github.com/user-attachments/assets/5c467484-392d-4bef-ba41-f93d339fbebe)

### Probem la connexió a la base de dades
Des d'un terminal amb un usuari sense privilegis hem de ser capaços de connectar introduïnt la nostra contrassenya.

```bash
mysql -u usuario -p
```
![image](https://github.com/user-attachments/assets/1190f546-3d17-4c48-9549-b6edb9d76c06)
_____________________________________________________________________________________________________________________________________________________________________________________________________________
## Descarreguem els fitxers de l'aplicació web
Anem al directori `/var/www/html` i descomprimim allà els fitxers de l'aplicació web, heu de substituir `app-web.zip` per el nom del vostre fitxer que heu descarregat amb l'aplicació web i el nom de la carpeta `app-web` per la carpeta que us ha creat, si la vostra instal·lació de linux està en un idioma diferent al català, no tindreu la carpeta `Baixades`, modifiqueu la comanda per adaptarla a les vostrs necessitats.

```bash
sudo cp ~/Baixades/app-web.zip /var/www/html
```
![image](https://github.com/user-attachments/assets/2f7bea47-5b1b-4de8-83a5-e955fc29214d)

Aneu al directori `/var/www/html`
```bash
cd /var/www/html
```
![image](https://github.com/user-attachments/assets/87d5757e-b3af-4b20-b807-924808243cab)

Descomprimiu el fitxer que heu baixat
```bash
sudo unzip app-web.zip
```
![image](https://github.com/user-attachments/assets/cc957069-9038-4325-b1f7-83d7902410bf)

Copieu els fitxers a la carpeta `/var/www/html`, modifiqueu `app-web` pel nom del directori on s'ha descomprimit el vostre arxiu.
```bash
sudo cp -R app-web/. /var/www/html
```
![image](https://github.com/user-attachments/assets/40be8751-c07c-40b7-8a1a-ec2106ccd574)

Eliminem la carpeta creada quan hem fet l'`unzip`
```bash
sudo rm -rf app-web/
```
![image](https://github.com/user-attachments/assets/48223bb7-090c-467b-89ba-225156913b99)

## Eliminem el fitxer `index.html` de l'`apache2`
```bash
sudo rm -rf /var/www/html/index.html
```
![image](https://github.com/user-attachments/assets/3ddce0c2-09b8-4c5a-8863-1948a8be0ada)

_____________________________________________________________________________________________________________________________________________________________________________________________________________
## Aplicació de permisos a les nostres aplicacions web
Un cop descomprimits els fitxers de l'aplicació web al directori `/var/www/html`, apliquem els següents permisos al directori `/var/www/html`

```bash
cd /var/www/html
```
![image](https://github.com/user-attachments/assets/4e79c2dc-2952-4968-9cdb-040095eef145)

```bash
sudo chmod -R 775 .
```
![image](https://github.com/user-attachments/assets/1ecf79ab-ef83-4043-af17-38d0312dd9cc)

```bash
sudo chown -R usuario:www-data .
```
![image](https://github.com/user-attachments/assets/0db1a48b-847a-489f-9076-45649f47cbe9)

## Accedim al navegador per veure que tot funciona
Poseu la direcció http://localhost al navegador web i configureu la cloud.
![image](https://github.com/user-attachments/assets/202401fa-63cc-4aff-9489-16b7ee9f61ac)

Ara tindras que posar això en el configuració


* **usuari:** usuario
* **contrasenya:** password
* **base de dades:** bbdd
* **domini:** localhost

![image](https://github.com/user-attachments/assets/5dde0d51-0466-430d-bc14-72b04724413e)


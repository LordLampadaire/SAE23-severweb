# Mise en place application web sur VM

#### Mise en place des éléments de base

Dans cette parite vous allez installer les paquets nécessaire à la mise en place notre serveur web.

pour cela vous aurez besion de 

- python version 3 et plus 

- gunicorn 

- nginx

- Django

![](/home/par_defaut/.config/marktext/images/2022-06-13-10-46-15-image.png)Nginx = serveur web 

Gunicorn = wsgi ( traducteur web <---> python)

##### Étape 1 installez nginx

en super utllisateur 

```
apt install nginx
```

##### Étape 2 installez l'environement virtuelle pour python

```
apt install -y python3-venv
```

 bravo vous avez installer les éléments de bases pour votre serveur web

### 

### Création de environement de travail et paramètrage

#### Configuration de Django

retournez en utilisateur simple

##### Étape 1 créez un environnement pour Django

```
python3 -m venv "votre nom d'environnemnt"
```

##### Étape 2 lancez votre environnement Django

```
source "votre nom d'environnement"/bin/activate
```

##### Étape 3 installez django

```
pip install django
```

##### Étape 4 installez gunicorn

```
pip install gunicorn
```

##### Étape 5 faites un git-clone de votre projet

```
git clone {l'URL du répertoire}
```

##### Étape 6.1 allez dans le fichier settings.py de votre projet

```
nano "nom du projet"/"nom du projet"/settings.py
```

##### Étape 6.2 copiez l'adresse ip de votre machine dans la ligne

```
ALLOWED_HOSTS = ['"votre adresse IP"']
```

sauvgardez

#### Configuration de gunicorn

##### Étape 1 créez un dossier  de configuration

```
mkdir conf
```

##### Étape 2.1 créez un fichier de configuration pour gunicorn

```
nano conf/gunicorn_config.py
```

##### Étape 2.2 recopiez les lignes suivantes dans votre fichier

```
command = '/home/"le nom de la machine"/django_env/bin/guincorn'
pythonpath = '/home/"le nom de votre machine"/"le nom de votre projet"'
bind = '"votre adresse IP":8000'
workers = 3
```

sauvgarder votre fichier 

##### Étape 3 lancer gunicorn

```
gunicorn -c conf/gunicorn_config.py "le nom de votre projet".wsgi
```

arrétez gunicorn et méttez le en arrière plan avec **bg** 

#### Configuration nginx

toujours dans l'environement virtuelle 

##### Étape 1 Lancez nginx

```
sudo service nginx start
```

##### Étape 2 créez un dossier static

```
mkdir static
```

##### Étape 3.1 allez dans votre ficher settings.py

```
nano "le nom de votre projet"/"le nom de votre projet"/settings.py
```

##### Étape 3.2 allez à la ligne STATIC_URL et changez le path

```
STATIC_URL = '/home/"le nom de la machine"/static'
```

sauvgardez

##### Étape 4.1 créez un fichier de configuration de votre projet pour nginx

```
sudo nano /etc/nginx/sites-avaible/"le nom de votre projet"
```

##### Étape 4.2 ajoutez les lignes suivantes

```
server {
    listen 80;
    server_name "votre adresse IP";

location /static/ {
    root /home/"le nom de la machine"/static/;
}
location /{
    proxy_pass htttp://"votre adresse IP":8000;
    }
}
```

##### Étape 5 allez dans le répertoitre /etc/nginx/sites-enabled/

```
cd /etc/nginx/sites-enabled/
```

##### Étape 6 mettez en place votre un lien vers votre projet

```
sudo ln -s /etc/nginx/sites-avaible/"le nom de votre projet"
```

##### Étape 7 redémarez nginx

```
sudo systemctl restart ngnix
```

##### Étape 8 allez sur votre navigateur et tapez l'adresse IP de la machine

votre serveur web est intallé et configuré 

- Create an apache2 subdomain

```
cd etc/

cd apache2/

cd sites-available/
```

- 000-default.conf

```shell
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

      <Directory /var/www/html>
                Options Indexes FollowSymLinks
	        AllowOverride All
                Require all granted
		Header set Access-Control-Allow-Origin "*"
        </Directory>
	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

exemple : 

emacs 001-monsite.conf

<VirtualHost *:80>
        DocumentRoot /var/www/html/projets/dossier du site/
        ServerName nomdusite.mennad-sekour.fr

        <Directory /var/www/html/projets/dossier du site/>
                 Options -Indexes +FollowSymlinks
                 AllowOverride All
        </Directory>
</VirtualHost>

```

- With laravel project

```
<VirtualHost *:80>
        DocumentRoot /var/www/html/projets/dossier du site/public
        ServerName nomdusite.mennad-sekour.fr

        <Directory /var/www/html/projets/dossier du site/public>
                 Options -Indexes +FollowSymlinks
                 AllowOverride All
        </Directory>
</VirtualHost>

sudo chmod 777 -R storage in project folder
```

- With symfony project

```
<VirtualHost *:80>
    ServerName nomdusite.mennad-sekour.fr
    ServerAlias nomdusite.mennad-sekour.fr

    DocumentRoot /var/www/html/projets/PHP_Avance_my_quiz/web
    <Directory /var/www/html/projets/PHP_Avance_my_quiz/web>
        AllowOverride All
        Order Allow,Deny
        Allow from All
    </Directory>

    # uncomment the following lines if you install assets as symlinks           
    # or run into problems when compiling LESS/Sass/CoffeScript assets          
    # <Directory /var/www/project>                                              
    #     Options FollowSymlinks                                                
    # </Directory>                                                              

    ErrorLog /var/log/apache2/project_error.log
    CustomLog /var/log/apache2/project_access.log combined
</VirtualHost>

Don't forget to delete all cache !

rm -rf app/cache/*
rm -rf app/logs/*
```

- activate the file

```
sudo a2ensite nom_du_fichier.conf
service apache2 restart
```

- Prohibit an ip pointing to our ip

```
/etc/apache2/sites-available# emacs 000-default.conf 

RewriteEngine on
RewriteCond %{HTTP_HOST} !^www.mennad-sekour.fr$
RewriteRule ^/?(.*) http://www.mennad-sekour.fr/$1 [QSA,R=301,L]
```

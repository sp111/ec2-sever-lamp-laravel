# ec2-sever-lamp-laravel
Steps for server setup with LAMP stack and laravel

Setup EC2 instance:
Login to aws console
Navigate to EC2 service
Click on “Launch Instance”. This will launch a setup wizard. Set configurations according to your requirements and launch instance. 
It will ask to generate a key pair, which will be useful SSH connection to server.
( You can skip EC2 instance setup, if you are not woking with aws EC2 instance)
Then login to ssh terminal using that key
	
	ssh -i {key file location } ubuntu@{server public address} (Linux/Mac)

Note: before connecting to ssh, set your keyfile permission to 400.
 
 	sudo chmod -R 400 { key file location } 
 
Setup LAMP for laravel project

Follow this link for setup
	https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04

Install required php packages

Laravel requires mbstring and php-xml package

	sudo apt-get install php-mbstring
	sudo apt-get install php-xml
	sudo apt install zip unzip php7.0-zip

Install composer
		
		sudo apt install composer

Copy your project to apache root directory via ftp or do a git clone of your project. Navigate to your project. And run composer install.

	cd {your project path}
	sudo composer install
	
Set permissions for laravel publically accessible folders.

	sudo chmod -R 770 public
	sudo chmod -R 770 bootstrap/cache
	sudo chmod -R 770 storage

Set root in apache config
Open your apache config file in folder /etc/apache2/sites-available. You can use default config file or create new. Following is example apache config file.

	<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html/my-project-platform/public

        <Directory /var/www/html/my-project-platform>
                AllowOverride All
        </Directory>

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
	</VirtualHost>

Enable rewrite module for laravel routing work.

	sudo a2enmod rewrite

Restart Apache server and access your site with public IP.

	sudo systemctl restart apache2

That's all. You are ready to go :)

# Overview
Dwarfy is a URL shortener website application.

This project is configured to be deployed using either PHP's built-in web server or Apache2. 

View [Installation](#installation) for more information

## Installation
This project is originally intended to run with Apache2, PHP 7.4, MySQL, CodeIgniter 3.1.13 and the required dependencies necessary for all these to work with each other.
1. Clone the project.
	```sh 
	git clone https://github.com/Miyukiin/Dwarfy.git
 	```
2.  Install PHP 7.4, Apache2, MySQL and common dependencies
 	```sh 
 	sudo apt update
    sudo apt install libapache2-mod-php mysql-server # Apache PHP Module, MySQL Server
    sudo add-apt-repository ppa:ondrej/php # Repository containing multiple PHP versions.
    sudo apt install php7.4 php7.4-cli php7.4-common php7.4-mysql php7.4-curl php7.4-mbstring php7.4-xml php7.4-zip # Install PHP 7.4 and dependencies
	```
3. Ensure you have the right version enabled for PHP.
    ```sh
    sudo update-alternatives --config php # Select PHP 7.4
    ```
4. Configure Apache2 to use the right PHP version
    ```sh
    sudo a2dismod phpx.x # If you have a another PHP version already enabled.
    sudo a2enmod php7.4 # Enable PHP 7.4
    systemctl restart apache2 # Restart Apache2 to use new configuration.
    ```
5. Create and configure the project's top-most `.htaccess` file.
	```sh 
	<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /dwarfy/ 

    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^(.*)$ index.php/$1 [L]
	</IfModule>


	# RewriteEngine On: Enables the runtime rewriting engine.#
	# RewriteCond !-f and !-d: Tells Apache that if the requested URL isn't an actual file or folder on the server, it should pass the request to index.php.
	# RewriteRule: Redirects everything to index.php while keeping the URI segments intact.
	```
6. Create and Configure `dwarfy.conf` in `/etc/apache2/sites-available`
	```sh 
	<VirtualHost *:80>
        ServerName dwarfy.local
        DocumentRoot /var/www/dwarfy

        <Directory /var/www/dwarfy>
            AllowOverride All
            Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/dwarfy_error.log
        CustomLog ${APACHE_LOG_DIR}/dwarfy_access.log combined
    </VirtualHost>
	```
7. Add an entry for `dwarfy.local` in `etc/hosts` or `C:\Windows\System32\drivers\etc\hosts` depending on your operating system.
    ```sh
    127.0.0.1 dwarfy.local
    ```
8. Use our configuration.
    ```sh
    sudo a2ensite dwarfy.conf
    sudo systemctl reload apache2
    ```
9. Run `a2enmod` rewrite to allow our .htaccess file to work.
	```sh 
	sudo a2enmod rewrite
	sudo systemctl restart apache2
	```
10. Modify `etc/apache2/ports.conf` to allow WSL2 Apache to listen not only to Linux VM's internal IP, but all network interfaces.
    ```sh
    sudo nano ports.conf # Change Listen 80 to *:80
    ```
11. Run the project either via PHP's built-in web server or Apache.
	```sh
	# If running PHP's built-in web server
	php -S localhost:3000
	```
12. Visit the project's URL in the browser.
	```sh 
	PHP: http://localhost:3000/
	Apache: http://dwarfy.local
	```
 

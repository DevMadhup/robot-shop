## Ratings microservice
### <mark>FROM php:7.4-apache:</mark> 
- This line of code tells Docker to pull a pre-built image that comes with PHP and version 7.4 as well as an Apache Web Server installed on it. This is our container base image.
#
### <mark>RUN apt-get update && apt-get install -yqq unzip libzip-dev && docker-php-ext-install pdo_mysql opcache zip:</mark>
- `apt-get update:` 
Refreshes the package list, this is something manually done by running apt-get update.
- `apt-get install -yqq unzip libzip-dev` the installs (unzipneeded to extract files and )lib_zip.dev which is needed for PHP zip handling.
- `docker-php-ext-install pdo_mysql opcache zip` pdo_mysql is being installed so that PHP can connect to MySQL databases.
#
### <mark>RUN echo "instana.enable_auto_profile=1" > "/usr/local/etc/php/conf.d/zzz-instana-extras.ini"</mark>
- This line creates a configuration file that enables a feature called AutoProfile for PHP. This is useful for monitoring and profiling PHP applications.
#
### <mark>COPY status.conf /etc/apache2/mods-available/status.conf</mark>
- `COPY status.conf /etc/apache2/mods-available/status.conf` copies your custom **status.conf** file into the Apache configuration directory inside the container.
#
### <mark>RUN a2enmod rewrite && a2enmod status</mark>
- `a2enmod rewrite && a2enmod status` turns on the rewrite module (used for URL rewriting) and the status module (provides server status information).
#
### <mark>WORKDIR /var/www/html</mark>
- This line sets the working directory for the next commands. It means that any files we copy or commands we run will be relative to /var/www/html, the default folder where Apache looks for web content.
#
### <mark>COPY html/ /var/www/html</mark>
- This line copies all the files from the html folder on your local machine to the /var/www/html folder inside the container. This is where your web application files go.
#
### <mark>COPY --from=composer /usr/bin/composer /usr/bin/composer</mark>
- `COPY --from=composer /usr/bin/composer /usr/bin/composer` copies the Composer executable (a tool for managing PHP dependencies) from another Docker image into our container.
#
### <mark>RUN composer install</mark>
- `RUN composer install` runs the Composer command to install all the PHP libraries your application needs, as listed in composer.json.
#
### <mark>RUN rm -Rf /var/www/var/* </mark>
- `rm -Rf /var/www/var/*` deletes everything inside the /var/www/var directory to ensure it's clean.
#
### <mark> RUN chown -R www-data /var/www </mark>
- `chown -R www-data /var/www` changes the ownership of all files in /var/www to www-data, which is the default user that Apache runs as.
#
### <mark> RUN chmod -R 777 /var/www</mark> </mark>
- `chmod -R 777 /var/www` gives full read, write, and execute permissions to everyone for all files in /var/www. This is usually not recommended for production due to security concerns, but it's used here to ensure that the application can run without permission issues.
#


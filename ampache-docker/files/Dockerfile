FROM debian:10

LABEL maintainer Sandor Jokai <shanxman1986@gmail.com>

RUN apt update && apt install -y --no-install-recommends apache2 php7.3 \
php7.3-mysql php7.3-curl libapache2-mod-php7.3 ffmpeg nano wget unzip ca-certificates supervisor \
&& mkdir -p /var/log/supervisor && mkdir -p /etc/supervisor/conf.d

COPY ampache.conf /etc/apache2/sites-available/

RUN a2enmod rewrite && a2dissite 000-default.conf && a2ensite ampache.conf

RUN mkdir -p /var/www/ampache && cd /var/www/ampache && \
wget http://github.com/ampache/ampache/releases/download/4.1.1/ampache-4.1.1_all.zip && unzip \
ampache*all.zip && chown -R www-data:www-data /var/www/ampache && mv rest/.htaccess.dist \
rest/.htaccess && mv play/.htaccess.dist play/.htaccess && mv channel/.htaccess.dist channel/.htaccess

COPY supervisord.conf /etc/supervisor/conf.d

VOLUME ["/var/lib/docker/volumes/ampache"]

EXPOSE 80

CMD ["/usr/bin/supervisord"]

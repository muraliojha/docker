FROM ubuntu:18.04

RUN apt-get update -y --fix-missing
RUN apt-get install -y software-properties-common

#RUN apt-get install -y python-software-properties
RUN LC_ALL=C.UTF-8 add-apt-repository -y ppa:ondrej/php
RUN apt-get update -y --fix-missing
RUN apt-get install nginx -y
COPY config/nginx.conf.sample /var/www/nginx.conf.sample
COPY config/default /etc/nginx/sites-enabled/default
RUN apt-get update -y --fix-missing
RUN apt-get install -y php7.4-cli
#COPY config/ioncube_loader_lin_7.3.so /usr/lib/php/20180731/ioncube_loader_lin_7.4.so
RUN apt-get install -y php7.4-xml php7.4-mbstring php7.4-mysqli php7.4-intl php7.4-gd php7.4-mysql php7.4-zip php7.4-soap php7.4-fpm php7.4-curl php7.4-bcmath
COPY config/php.ini /etc/php/7.4/cli/php.ini
COPY config/php.ini /etc/php/7.4/fpm/php.ini
RUN apt-get install curl -y
COPY composer /usr/local/bin/composer
RUN chmod +x /usr/local/bin/composer
#RUN curl -s https://getcomposer.org/installer | php
#RUN mv composer.phar /usr/local/bin/composer
#RUN a2enmod rewrite
RUN apt-get install redis-server -y
#COPY htdocs /var/www/html
#RUN cd /var/www/html && composer install
#RUN composer update
RUN service php7.4-fpm restart
RUN service nginx restart
RUN apt-get install supervisor -y
RUN apt-get install vim -y
RUN apt-get install git -y
COPY config/supervisord.conf /etc/supervisor/conf.d/
CMD ["/usr/bin/supervisord", "-n"]
#RUN apt-get update
WORKDIR /var/www/html

FROM  docker.akson.ru/php7.4:28

LABEL maintainer="akson.ru <docker@akson.ru>"

RUN pecl channel-update pecl.php.net && \
            chmod -R 0777 /var/tmp/ && \
            apk add nginx supervisor \
                       &&  rm -rf /tmp/pear \
                       && rm -f /etc/nginx/conf.d/default.conf \
                       && echo "upstream php-upstream { server localhost:9001; }" > /etc/nginx/conf.d/upstream.conf

ADD ./docker/nginx.conf /etc/nginx/
ADD ./docker/symfony.conf /etc/nginx/conf.d

###################### DEBUG #######################
#RUN rm -f /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini
#ADD ./docker/06_xdebug.ini /usr/local/etc/php/conf.d/
#RUN wget http://xdebug.org/files/xdebug-2.9.6.tgz \
#    && tar xfz xdebug-2.9.6.tgz \
#    && cd xdebug-2.9.6/ \
#    && phpize \
#    && ./configure \
#    && make \
#    && cp modules/xdebug.so /usr/local/lib/php/extensions/no-debug-non-zts-20190902 \
#    && echo -e '\nzend_extension = /usr/local/lib/php/extensions/no-debug-non-zts-20190902/xdebug.so\n' >> /usr/local/etc/php/php.ini
###################### END OF DEBUG #######################

COPY ./symfony /var/www/symfony
WORKDIR /var/www/symfony
RUN php composer.phar install -vvv && chmod -R 0777 /var/www/symfony && chown -R www-data:www-data /var/www/symfony

ADD ./docker/supervisord.conf /etc/
ENTRYPOINT ["/usr/bin/supervisord",  "-c",  "/etc/supervisord.conf"]
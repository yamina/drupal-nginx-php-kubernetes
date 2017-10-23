ARG PHP_FPM_VERSION=latest
ARG REGISTRY_NAMESPACE=orod

FROM registry.ng.bluemix.net/${REGISTRY_NAMESPACE}/config-php-fpm:${PHP_FPM_VERSION}

ARG DRUPAL_VERSION
ARG DRUPAL_MD5

RUN curl -sS https://getcomposer.org/installer | \
    php -- --install-dir=/usr/bin/ --filename=composer

COPY tmp/composer.json ./

# COPY tmp/composer.lock ./

RUN composer install --no-scripts --no-autoloader

COPY . ./


RUN rm -fr /var/www/html
RUN composer create-project drupal-composer/drupal-project:8.x-dev /var/www/html/ --stability dev --no-interaction

ADD tmp/* /var/www/html/


# RUN composer dump-autoload --optimize && composer run-script post-install-cmd

COPY start.sh /root/
RUN chmod +x /root/start.sh

EXPOSE 9000

ENTRYPOINT ["/root/start.sh"]
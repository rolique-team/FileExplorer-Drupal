FROM wodby/drupal-php:7.1-3.0.0

MAINTAINER Igor Semenyuk <igor.semenyuk@rolique.io>

ENV workdir /var/www/html
ENV PHP_FPM_REQUEST_SLOWLOG_TIMEOUT 0.1
ENV PHP_XDEBUG_DEFAULT_ENABLE 1
ENV PHP_XDEBUG_REMOTE_AUTOSTART 1
ENV PHP_XDEBUG 1
ENV PHP_XDEBUG_REMOTE_HOST 172.17.0.1
ENV PHP_IDE_CONFIG serverName=localhost
ENV PHP_XDEBUG_REMOTE_CONNECT_BACK 1

USER root

RUN apk --no-cache add curl

# persistent / runtime deps
ENV PHPIZE_DEPS \
    autoconf \
    file \
    g++ \
    gcc

RUN set -xe \
    && apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS

RUN curl -OL https://github.com/squizlabs/PHP_CodeSniffer/releases/download/2.9.1/phpcbf.phar \
 && mv phpcbf.phar /usr/bin/phpcbf

RUN curl -OL https://github.com/squizlabs/PHP_CodeSniffer/releases/download/2.9.1/phpcs.phar \
	&& mv phpcs.phar /usr/bin/phpcs

RUN chmod a+x /usr/bin/phpc*

USER www-data

RUN composer global require drupal/coder:^8.2.12 \
	&& phpcs --config-set installed_paths /home/www-data/.composer/vendor/drupal/coder/coder_sniffer

WORKDIR ${workdir}

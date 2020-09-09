FROM php:7.4-fpm

MAINTAINER Albert

RUN pecl install swoole && docker-php-ext-enable swoole

RUN curl -fSL http://resources.swoole-cloud.com/getClient/trial/2.9.2 -o swoole_tracker.tar.gz \
    && mkdir -p swoole_tracker \
    && tar -xvf swoole_tracker.tar.gz -C swoole_tracker \
    && cd ./swoole_tracker \
    && echo 'NONE' > ./app_deps/node-agent/magicstring \
    && cp swoole_tracker74.so /opt/swoole_tracker.so \
    && printf 'extension=/opt/swoole_tracker.so\napm.enable=1\napm.sampling_rate=100\napm.enable_memcheck=1\n' > /usr/local/etc/php/conf.d/swoole-tracker.ini \
    && exec ./deploy_env.sh

RUN rm ./swoole_tracker.tar.gz && rm -rf ./swoole_tracker

RUN printf '#!/bin/sh\n/opt/swoole/script/php/swoole_php /opt/swoole/node-agent/src/node.php &\nphp-fpm $@' > /opt/swoole/entrypoint.sh && \
    chmod 755 /opt/swoole/entrypoint.sh

ENTRYPOINT [ "sh", "/opt/swoole/entrypoint.sh" ]

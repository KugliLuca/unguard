
#Copyright 2023 Dynatrace LLC
#Licensed under the Apache License, Version 2.0 (the "License");
#you may not use this file except in compliance with the License.
#You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
#Unless required by applicable law or agreed to in writing, software
#distributed under the License is distributed on an "AS IS" BASIS,
#WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#See the License for the specific language governing permissions and
#limitations under the License.

FROM php:7.3-cli

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www

RUN apt update

RUN apt -y install git

ENV COMPOSER_ALLOW_SUPERUSER=1

COPY . /var/www

#This is faster https://github.com/composer/composer/issues/8205#issuecomment-507256979
RUN php /usr/bin/composer config --global repos.packagist composer https://packagist.org

RUN php /usr/bin/composer update --no-cache

RUN php /usr/bin/composer install

EXPOSE 8000
#Fixes error while connecting since laravel requires a key
RUN php artisan --env=local key:generate

#Fixes error while accessing enviroment variables
RUN php artisan config:clear

#MySqlDriver
RUN docker-php-ext-install mysqli pdo pdo_mysql;

RUN php artisan --env=local optimize:clear

RUN php /usr/bin/composer dump-autoload

RUN php /usr/bin/composer clear-cache

CMD php artisan --env=local make:database && php artisan migrate:fresh --force && php -S 0.0.0.0:8000 -t public

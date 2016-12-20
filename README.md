## Описание

Это Dockerfile, позволяющие собрать простой образ для Docker с PHP-FPM. Имеется возможность изменения параметров PHP, доступно три версии PHP 7.1, 7.0 и 5.6.

### PHP собран с поддержкой следующих модулей

 - **Расширения, включенные по умолчанию:** gd, mysqli, pdo, pdo_mysql, intl, dom, xml, xsl, xmlrpc, zip, bz2, fileinfo, curl, iconv, json, soap, calendar, gettext, mcrypt, tidy, phar, gettext, mysql (доступно только для php 5.6), imagick
 - **Расширения, отключенные по умолчанию:** mongodb, gearman, opcache, memcached , redis, imap, ldap, xcache (доступно только для PHP 5.6)

## Репозиторий Git

Репозиторий исходных файлов данного проекта: [https://github.com/MiraFox/php-fpm](https://github.com/MiraFox/php-fpm)

## Репозиторий Docker Hub

Расположение образа в Docker Hub: [https://hub.docker.com/r/mirafox/php-fpm/](https://hub.docker.com/r/mirafox/php-fpm/)

## Версии

|   Tag  |  PHP   |
|--------|--------|
| latest | 7.1.0  |
|  7.1   | 7.1.0  |
|  7.0   | 7.0.14 |
|  5.6   | 5.6.29 |

## Использование Docker Hub

```
sudo docker pull mirafox/php-fpm
```

## Запуск

```
docker run -d --name myapp \
    -v /home/username/sitename/www/:/var/www/html/ \
    mirafox/php-fpm

docker run -d -p 80:80 -p 443:443 \
    --link myapp:myapp \
    -v /home/username/sitename/www/:/var/www/html/ \
    -v /home/username/sitename/logs/:/var/log/nginx/ \
    mirafox/nginx
```

## Доступные параметры конфигурации

### Параметры изменяющие настройки PHP

| Параметр | Изменяемая директива | По умолчанию |
|----------|----------------------|--------------|
|**PHP_ALLOW_URL_FOPEN**| allow_url_fopen | On |
|**PHP_DISPLAY_ERRORS**| display_errors | Off |
|**PHP_MAX_EXECUTION_TIME**| max_execution_time | 30 |
|**PHP_MAX_INPUT_TIME**| max_input_time | 60 |
|**PHP_MEMORY_LIMIT**| memory_limit | 128M |
|**PHP_POST_MAX_SIZE**| post_max_size | 8M |
|**PHP_SHORT_OPEN_TAG**| short_open_tag | On |
|**PHP_TIMEZONE**| date.timezone | Europe/Moscow |
|**PHP_UPLOAD_MAX_FILEZIZE**| upload_max_filesize | 2M |

#### Пример использования

```
sudo docker run -d \
    -e 'PHP_TIMEZONE=Europe/Moscow' \
    -e 'PHP_MEMORY_LIMIT=512' \
    -e 'PHP_SHORT_OPEN_TAG=On' \
    -e 'PHP_UPLOAD_MAX_FILEZIZE=16' \
    -e 'PHP_MAX_EXECUTION_TIME=120' \
    -e 'PHP_MAX_INPUT_TIME=120' \
    -e 'PHP_DISPLAY_ERRORS=On' \
    -e 'PHP_POST_MAX_SIZE=32' \
    -e 'PHP_ALLOW_URL_FOPEN=Off' \
    mirafox/php-fpm
```

### Параметры подключения расширений PHP

 - **PHP_MODULE_MEMCACHED**: при установки в значение On подключается расширение memcached
 - **PHP_MODULE_OPCACHE**: при установки в значение On подключается расширение OPcache
 - **PHP_MODULE_XCACHE**: при установки в значение On подключается расширение XCache (только для версии PHP 5.6)
 - **PHP_MODULE_REDIS**: при установки в значение On подключается расширение Redis
 - **PHP_MODULE_MONGO**: при установки в значение On подключается расширение MongoDB
 - **PHP_MODULE_GEARMAN**: при установки в значение On подключается расширение Gearman
 - **PHP_MODULE_IMAP**: при установки в значение On подключается расширение IMAP
 - **PHP_MODULE_LDAP**: при установки в значение On подключается расширение LDAP

#### Пример использования

```
sudo docker run -d -e 'PHP_MODULE_MEMCACHED=On' -d mirafox/php-fpm
```

## Использование PHP Composer

```
docker exec -it <CONTAINER_NAME> /usr/local/bin/composer
```

## Использование собственных конфигурационных файлов

Вы можете использовать собственные конфигурационные файлы для php. Для этого Вам необходимо создать их в директории **/var/www/html/config/**. При их обнаружении, Ваши конфигурационные файлы будут скопированы и заменят существующие.

### PHP

 - **/var/www/html/config/php/php.ini** - данным файлом будет заменен /usr/local/etc/php/php.ini (при этом параметры, изменяющие настройки PHP, переданные при запуске контейнера, будут игнорированы)
 - **/var/www/html/config/php/pool.conf** - данным файлом будет заменен /usr/local/etc/php-fpm.d/www.conf


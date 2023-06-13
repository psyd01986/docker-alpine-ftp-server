# docker-alpine-ftp-сервер
[![Docker Stars](https://img.shields.io/docker/stars/delfer/alpine-ftp-server.svg)](https://hub.docker.com/r/delfer/alpine-ftp -server/) [![Docker Pulls](https://img.shields.io/docker/pulls/delfer/alpine-ftp-server.svg)](https://hub.docker.com/r/delfer /alpine-ftp-server/) [![Автоматизированная сборка Docker](https://img.shields.io/docker/automated/delfer/alpine-ftp-server.svg)](https://hub.docker. com/r/delfer/alpine-ftp-server/) [![Статус сборки Docker](https://img.shields.io/docker/build/delfer/alpine-ftp-server.svg)](https:/ /hub.docker.com/r/delfer/alpine-ftp-server/) [![MicroBadger Layers](https://img.shields.io/microbadger/layers/delfer/alpine-ftp-server.svg)] (https://hub.docker.com/r/delfer/alpine-ftp-server/) [![Размер MicroBadger](https://img.shields.io/microbadger/image-size/delfer/alpine-ftp -server.svg)](https://hub.docker.com/r/delfer/alpine-ftp-сервер/)  
Небольшой и гибкий образ докера с сервером vsftpd

## Использование
```
докер запустить -d \
    -р 21:21\
    -p 21000-21010:21000-21010 \
    -e ПОЛЬЗОВАТЕЛИ="один|1234" \
    -e АДРЕС=ftp.site.domain \
    delfer/alpine-ftp-сервер
```

## Конфигурация

Переменные среды:
- `ПОЛЬЗОВАТЕЛИ` - список, разделенный пробелами и `|` (необязательно, по умолчанию: `alpineftp|alpineftp`)
  - формат `имя1|пароль1|[папка1][|uid1][|gid1] имя2|пароль2|[папка2][|uid2][|gid2]`
- `АДРЕС` - внешний адрес, по которому клиенты могут подключаться к пассивным портам (необязательно, должен разрешаться в IP-адрес ftp-сервера)
- `MIN_PORT` - минимальный номер порта, который будет использоваться для пассивных подключений (необязательно, по умолчанию `21000`)
- `MAX_PORT` - максимальный номер порта, который будет использоваться для пассивных подключений (необязательно, по умолчанию `21010`)

## ПОЛЬЗОВАТЕЛИ примеры

- `пользователь|пароль foo|bar|/home/foo`
- `пользователь|пароль|/home/пользователь/каталог|10000`
- `пользователь|пароль|/home/пользователь/каталог|10000|10000`
- `пользователь|пароль||10000`
- `пользователь|пароль||10000|82`: добавить в существующую группу (www-данные)

## FTPS (протокол передачи файлов + SSL) Пример

Выпустите бесплатный сертификат Let's Encrypt и используйте его с `alpine-ftp-server`.

```
mkdir -p /etc/letsencrypt
докер запустить -it --rm \
    -р 80:80 \
    -v "/etc/letsencrypt:/etc/letsencrypt" \
    certbot/certbot точно \
    --автономный \
    --предпочтительные вызовы http \
    -n --согласиться с \
    --email i@delfer.ru \
    -d ftp.сайт.домен
докер запустить -d \
    --имя фтп \
    -р 21:21\
    -p 21000-21010:21000-21010 \
    -e ПОЛЬЗОВАТЕЛИ="один|1234" \
    -e АДРЕС=ftp.site.domain \
    -e TLS_CERT="/etc/letsencrypt/live/ftp.site.domain/fullchain.pem" \
    -e TLS_KEY="/etc/letsencrypt/live/ftp.site.domain/privkey.pem" \
    delfer/alpine-ftp-сервер
```

- Не забудьте заменить ftp.site.domain на фактический домен, указывающий на IP вашего сервера.
- Убедитесь, что у вас есть доступный порт 80 для автономного режима certbot для выдачи сертификата.
- Не забудьте обновить сертификат через 3 месяца с помощью команды `certbot renew`.
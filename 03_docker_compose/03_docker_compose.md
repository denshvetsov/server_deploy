пример docker-composer.yaml
зависит 
 от .env файла 
 от nginx/

все три файла/папки должны быть скопированы в домашнюю директорию пользователя
скрипт github actions будет забирать от туда


version: '3.8'

services:
  db:
    image: postgres:13.0-alpine
    volumes:
      - /var/lib/postgresql/data/
    env_file:
      - ./.env
  web:
    image: denshvetsov/yamdb_final:latest
    restart: always
    volumes:
      - static_value:/app/static/
      - media_value:/app/media/
    depends_on:
      - db
    env_file:
      - ./.env

  nginx:
    image: nginx:1.21.3-alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
      - static_value:/var/html/static/
      - media_value:/var/html/media/
    depends_on:
      - web

volumes:
  static_value:
  media_value:
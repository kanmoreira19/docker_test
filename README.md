# Docker Test
## LAB 6: Imagem Docker Otimizada
O LAB 6 foi implementado com multi-stage build, .dockerignore, usuário non-root e labels padrão, validando práticas de otimização Docker.

Dockerfile implementado:

```text
FROM node:22-bookworm AS build

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build  

FROM debian:12

RUN apt-get update && apt-get install -y apache2 && apt-get clean
ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="www-data"
ENV APACHE_RUN_GROUP="www-data"
ENV APACHE_LOG_DIR="/var/log/apache2"

COPY index.html /var/www/html/

LABEL title="Apache_Hexxa"
LABEL description="webserver"
LABEL version="1.0.0"
LABEL authors="Kleyton de Andrade <kanmoreira@gmail.com"

RUN mkdir -p /var/www/html /var/log/apache2 /var/run/apache2 \
    && chown -R www-data:www-data /var/www/html /var/log/apache2 /var/run/apache2 \
    && echo "ServerName localhost" >> /etc/apache2/apache2.conf


USER www-data

VOLUME /var/www/html/
EXPOSE 80

ENTRYPOINT ["/usr/sbin/apachectl"]
CMD ["-D","FOREGROUND"]
```

.dockerignore (essencial para otimização):

```text
node_modules
npm-debug.log
.git
README.md
.env
Dockerfile
.dockerignore
```

Comandos executados:

```bash
docker build -t hexxa_test_docker:1.0.0 .

docker run -p 8080:80 hexxa_test_docker:1.0.0
```
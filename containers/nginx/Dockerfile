FROM nginx:alpine

# Copia a configuração para o container
COPY nginx.conf /etc/nginx/

# Adiciona pacotes
RUN apk update \
    && apk upgrade \
    && apk add --no-cache bash \
    && apk add --no-cache curl

# Cria o grupo e usuario www-data dentro do container
RUN set -x ; \
    addgroup -g 82 -S www-data ; \
    adduser -u 82 -D -S -G www-data www-data

# Remove a confiuguração default
RUN rm /etc/nginx/conf.d/default.conf

# Expõe as portas 80 e 443
EXPOSE 80 443
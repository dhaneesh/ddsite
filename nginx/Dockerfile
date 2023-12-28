FROM nginx:alpine

COPY nginx.conf /etc/nginx/conf.d

RUN set -eux; \
    rm -rf /etc/nginx/conf.d/default.conf; \
    mkdir -p /var/www; \
	ln -sf /opt/drupal/web /var/www/html

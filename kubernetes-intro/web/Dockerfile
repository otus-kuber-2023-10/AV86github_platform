FROM nginx:1.25.3

RUN usermod -u 1001 nginx

WORKDIR /app
COPY nginx.conf /etc/nginx/nginx.conf
COPY homework.html ./
RUN touch /var/run/nginx.pid && chown 1001:101 /var/run/nginx.pid
RUN chown -R 1001:101 /docker-entrypoint.sh && chmod 777 /docker-entrypoint.sh
RUN chown -R 1001:101 /var/cache/nginx

USER nginx
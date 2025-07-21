FROM nginx:1.24
RUN rm -rf /usr/share/nginx/html/index.html
RUN rm -rf /etc/nginx/nginx.conf
COPY nginx /etc/nginx/nginx.conf
COPY static/ /usr/share/nginx/html/

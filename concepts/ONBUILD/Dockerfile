FROM almalinux:9
RUN dnf install nginx -y
RUN rm -rf /usr/share/nginx/html/index.html
ONBUILD COPY index.html /usr/share/nginx/html/index.html
CMD [ "nginx", "-g", "daemon off;" ]


## ONBUILD will not be executed will building this image. 
## It's applicable when this is used as an image
## It's like a condition, when this image is used as an buildimage(FROM) then they must have index.html file

##RUN, to install packages
## FROM, for base OS
## Daemon off to make nginx run on the foreground
FROM almalinux:9
RUN dnf install nginx -y   
CMD ["nginx", "-g", "daemon off;" ]    
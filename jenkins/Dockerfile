FROM ubuntu:latest
RUN apt-get update && apt-get install nginx -y
ARG W
COPY index.html /var/www/lib
RUN cat /var/www/html/index.html |grep -w Hello
CMD ["nginx", "-g", "daemon off;"]
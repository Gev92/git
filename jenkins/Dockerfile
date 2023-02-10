FROM ubuntu:latest
RUN apt-get update && apt-get install nginx -y
ARG W
COPY index.html /home/devops/
RUN cat /home/devops/index.html |grep -w Hello
CMD ["nginx", "-g", "daemon off;"]
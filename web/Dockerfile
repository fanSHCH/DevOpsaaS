FROM nginx:alpine

VOLUME /tmp
ADD html /var/html
COPY nginx.conf /etc/nginx/nginx.conf
CMD ["nginx", "-g", "daemon off;"]

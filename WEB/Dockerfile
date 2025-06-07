FROM nginx:stable-alpine
#COPY ./WEB/style.css /usr/share/nginx/html/style.css
COPY index.html /usr/share/nginx/html/index.html
#COPY ./WEB/script.js /usr/share/nginx/html/script.js
COPY images/ /usr/share/nginx/html/images

COPY assets/ /usr/share/nginx/html/assets

EXPOSE 80
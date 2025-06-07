FROM nginx:stable-alpine
COPY ./WEB /usr/share/nginx/html
#COPY ./WEB/style.css /usr/share/nginx/html/style.css
COPY ./WEB/index.html /usr/share/nginx/html/index.html
#COPY ./WEB/script.js /usr/share/nginx/html/script.js
COPY ./WEB/images/ /usr/share/nginx/html/images
COPY ./WEB/assets/ /usr/share/nginx/html/assets
EXPOSE 80
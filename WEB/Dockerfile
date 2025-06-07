FROM nginx:stable-alpine
COPY ./index.html /usr/share/nginx/html/


#COPY index.html /usr/share/nginx/html/index.html
#COPY ./WEB/script.js /usr/share/nginx/html/script.js
COPY images/ /usr/share/nginx/html/images

COPY assets/ /usr/share/nginx/html/assets

EXPOSE 80
FROM nginx
COPY . /usr/share/nginx/html/
EXPOSE 80
RUN docker build -t statische-pagina .
RUN --name statische-pagina -p 8080:80
CMD ["nginx", "-g", "daemon off;"]
# devops-project

para rodar statische-pagina
docker build -t statische-pagina .
docker run --name statische-pagina-container -d -p 8080:80 statische-pagina
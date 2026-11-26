# devops-project

para rodar statische-pagina
docker build -t statische-pagina .
docker run --name statische-pagina-container -d -p 8080:80 statische-pagina

ref poll-manager/server/Dockerfile --> https://medium.com/@anuragabcr/running-an-express-server-in-docker-8fa51fbd2b61

ref poll-manager/database/Dokerfile --> https://dev.to/vishak369/mysql-dockerfile-docker-container-1phb
--> https://hub.docker.com/_/mysql
--> https://dev.to/deborahemeni1/run-a-mysql-container-with-persistent-storage-using-docker-volumes-49ma
--> https://hub.docker.com/r/mysql/mysql-server
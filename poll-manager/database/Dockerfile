FROM mysql:8.0.23

ENV MYSQL_ROOT_PASSWORD=root

RUN mkdir /var/lib/mysql-files
RUN touch /var/log/mysqld.log
RUN chown mysql:mysql /var/log/mysqld.log

EXPOSE 3306
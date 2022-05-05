# Task-1.1---Docker
1.1
Создайте папку для проекта и перейдите в нее.
    1. mkdir project, cd project
    2. sudo apt install docker-ce
    3. docker run -p 80:80 nginx
    4. mkdir /var/lib/mysql
        docker run -d --name mysql-serv -p 3306:3306 -v /var/lib/mysql:/var/lib/mysql -e "MYSQL_ROOT_PASSWORD=passwdl" mysql
    5. apt-get install mysql-client
        mysql -h 127.0.0.1 -u root -p
        CREATE USER 'new'@'localhost' IDENTIFIED BY '123';
        CREATE DATABASE NEW_BASE;

1.2
    1. nano Dockerfile
    2. 
        

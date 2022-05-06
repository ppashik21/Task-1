# Task-1.1---Docker


1.1  Работа с Docker

    1.Создайте папку для проекта и перейдите в нее.
       mkdir project, cd project
       
    2.Установите докер на машину.
      sudo apt install docker-ce
      
    3. Запустите контейнер с nginx скачанным из docker hub. Контейнер должен быть запущен из командной строки с параметром -expose или -p для того чтобы после запуска          стартовая страница nginx была доступна из браузера вашего компьютера.
       docker run -p 80:80 nginx
       
    4. Запустите контейнер с MySQL скачанным из докер хаба, используйте --volume , -v при запуске контейнера для того чтобы сохранить базу данных жестком диске хоста.
       mkdir /var/lib/mysql
       docker run -d --name mysql-serv -p 3306:3306 -v /var/lib/mysql:/var/lib/mysql -e "MYSQL_ROOT_PASSWORD=passwdl" mysql
       
    5. После запуска контейнера подключитесь к базе, создайте нового пользователя и новую базу.
       apt-get install mysql-client
       mysql -h 127.0.0.1 -u root -p
       CREATE USER 'new'@'localhost' IDENTIFIED BY '123';
       CREATE DATABASE NEW_BASE;

1.2  Работа с Dockerfile

    1. Создайте файл Dockerfile в корневой папке проекта. 
       В качестве базового образа используйте Ubuntu 20.04
       nano Dockerfile
       FROM ubuntu:20.04
       
    2. Дополните Dockerfile инструкциями из которого при выполнении команды docker build соберется docker образ с установленным Ruby 2.7.2
        FROM ubuntu:20.04
        RUN apt update && apt install -y git curl autoconf bison build-essential \
            libdb-dev libffi-dev libgdbm6 libgdbm-dev libncurses5-dev \
            libreadline6-dev libssl-dev libyaml-dev  zlib1g-dev && rm -rf /var/lib/apt/lists/*
        RUN adduser --gecos '' --disabled-password ruby
        USER ruby
        ENV HOME /home/ruby
        ENV PATH $HOME/.rbenv/shims:$HOME/.rbenv/bin:$HOME/.rbenv/plugins/ruby-build/bin:$PATH
        RUN git clone https://github.com/rbenv/rbenv.git ~/.rbenv
        RUN git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
        RUN rbenv install 2.7.2
        RUN rbenv global 2.7.2
        
        docker build -t myCont .
    3. После успешной сборки образа, запустите контейнер для выполнения команды ruby -v, для проверки работоспособности ruby.
        docker run -it myCont
        ruby -v               

1.3 Работа с Docker Compose

    1.Установите docker-compose на хост
    sudo apt install docker-compose -y

    2. С помощью docker-compose установите и запустите сайт на Wordpress. Помимо docker-compose.yml файла у вас могут быть другие файлы? необходимые для работы          Wordpress, например nginx.conf и другие.
    version: '3'

services:
  mysql:
    image: mysql:8
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: wordpress
    volumes:
      - "./db:/var/lib/mysql"

  wordpress:
    image: wordpress:php7.4-apache
    ports:
      - "80:80"
    environment:
      WORDPRESS_DB_HOST: mysql
      WORDPRESS_DB_USER: root
      WORDPRESS_DB_PASSWORD: root
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - "./wp:/var/www/html/"

  wp-cli:
    image: wordpress:cli
    user: "33:33"
    environment:
      WORDPRESS_DB_HOST: mysql
      WORDPRESS_DB_USER: root
      WORDPRESS_DB_PASSWORD: root
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - "./wp:/var/www/html/"
      - "./configure-wp.sh:/opt/configure-wp.sh"
    command: "bash /opt/configure-wp.sh"
    
    docker-compose up -d

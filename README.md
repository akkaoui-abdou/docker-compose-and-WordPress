Docker Tutorial For Beginners - How To Containerize Wordpress Applications
===


Create an empty project directory.
---

```bash
$ mkdir my_wordpress
$ cd my_wordpress/

```

Create a docker-compose.yml file that starts your WordPress blog and a separate MySQL instance with volume mounts for data persistence:
---

```bash
$ touch docker-compose.yml
```

Content docker-compose.yml
---

```yml
version: "3.8"
    
services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    networks:
      - wpsite

  phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin
    container_name: phpmyadmin
    restart: always
    ports:
      - 8080:80
    links:
      - db
    environment:
      PMA_HOST: db
      PMA_PORT: 3306
      PMA_ARBITRARY: 1
    networks:
      - wpsite

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    volumes:
      - wordpress_data:/var/www/html
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    networks:
      - wpsite

networks:
  wpsite:
volumes:
  db_data: {}
  wordpress_data: {}
```

Build the project
---

Now, run docker compose up -d from your project directory.


```bash
$ docker compose up -d
```


Bring up WordPress in a web browser
---

Open http://localhost:8000 in a web browser.

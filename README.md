Docker Tutorial For Beginners - How To Containerize Wordpress Applications
===


Create an empty project directory.
---

```bash
mkdir my_wordpress
cd my_wordpress/

```

Create a docker-compose.yml file that starts your WordPress blog and a separate MySQL instance with volume mounts for data persistence:
---

```bash
touch docker-compose.yml
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
volumes:
  db_data: {}
  wordpress_data: {}
```

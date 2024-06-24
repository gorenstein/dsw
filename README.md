# Docker Symfony Workbench (DSW)

### How to check workbench
After start up workbench ``docker compose up -d``
1. Open default pages
   - Hello Word ``http://localhost:8080/hello-word/``
   - phpInfo ``http://localhost:8080/hello-word/phpinfo.php``
   - phpMyAdmin ``http://localhost:8088/``
   - adminer ``http://localhost:8089/?pgsql=postgres&username=root``

2. Check symfony requirements
   - ``docker compose exec php /bin/bash -c "symfony check:requirements"``

### How to create a new symfony app
#### In one step
- ``docker compose exec php /bin/bash -c "symfony new webapp --webapp && cd webapp && composer require symfony/apache-pack"``

#### Step by step
1. Open *php* container console  
    - ``docker compose exec -it php /bin/bash``

2. Run *symfony* command
   - ``symfony new webapp --webapp``
   - or ``symfony new webapp --webapp --version="5.4"``
   - or ``symfony new webapp --webapp --version="6.3.*"``
   - or ``symfony new webapp --webapp --version="7.*"``

3. Add *.htaccess* to project
   - ``cd webapp``
   - ``composer require symfony/apache-pack``

#### Check Symfony project installation
   - open ``http://localhost:8080/webapp/public/``
   - check logs ``docker compose logs -f``


## Common CLI Cheat Sheet
### docker compose
```
docker compose build
docker compose build --pull
docker compose build --pull --no-cache
```

```
docker compose up
docker compose up -d
docker compose up --build
docker compose up -d --build
```

```
docker compose stop
docker compose down
```

```
docker compose ls
docker compose ps
docker compose logs -f
```

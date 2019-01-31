# YII2APP DOCKER

Rapidly deploy LNMP services for Yii2 applications.

## Get

Make sure you have the docker package installed on your machine before you start, then continue to:

```bash
git clone https://github.com/hoybin/yii2app-docker.git docker
cd docker
```

## Configure

1. Create **.env** file for docker-compose

    ```bash
    cp .env-dist .env
    ```

    Modify it to set values of parameters, and use a [personal token](https://github.com/settings/tokens) you generate on github.com as value of **GITHUB_API_TOKEN** in the file.

2. Preset the database initialization script files

    *.sql files in **./mysql/initdb.d/** will be executed when mysql first start, please ensure the database info in these files are consistent with the settings in the .env file above.

3. Configure nginx virtual hosts

    Nginx virtual host configuration files is in **./nginx/conf.d/**, you can refer to [official recommended configuration](https://github.com/yiisoft/yii2/blob/master/docs/guide/start-installation.md#user-content-recommended-nginx-configuration-) for modifications.

## Startup

Okay, the trouble is over, just one line missing, let's get started:

```bash
docker-composer up -d
```

For the first time, it takes a little time, instead of having a cup of coffee and waiting for it to be finished.

## Deploy

Place the code in the `~/www/yii2app/` directory specified by variable `DOCUMENT_ROOT_PATH` in .env file. Or you can use composer to create a new project:

```bash
docker exec -it docker-yii2app_php_1_xxxxxxxxxxxx bash
composer create-project --prefer-dist yiisoft/yii2-app-advanced .
# composer is automatically installed...
exit # Don't forget to exit the container after completion
```

Then open the browser and visit the following addresses:

- frontend: http://yii2app.com:8080
- backend: http://admin.yii2app.com:8080
- phpmyadmin: http://localhost:8081

Ports 8080 and 8081 are already set in the .env file, and you should remember.

## Control

A few commands to know:

```bash
# stop services
docker-compose stop

# start services
docker-compose start

# restart services
docker-compose restart
```

More on docker-compose: https://docs.docker.com/compose

## Proxy

Maybe you need to access applications in containers through reverse proxy of nginx on host:

```bash
sudo cp nginx-proxy.conf /etc/nginx/conf.d/yii2app.conf
sudo nginx -ts reload
```

then access the following addresses in the browser:

- frontend: http://yii2app.com
- backend: http://admin.yii2app.com
- phpmyadmin: http://pma.yii2app.com

---

Congratulations! Your code is running ...

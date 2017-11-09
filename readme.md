Clone `frontend`, `backoffice`, `api`, `attachments`, `api client`, `docker` repositories to `newmood` folder from bitbucket.
It is best to have them in one folder.
```
cd ~
mkdir newmood
cd newmood
git clone git@bitbucket.org:recafe/membershop-api.git
git clone git@bitbucket.org:recafe/membershop-api-client.git
git clone git@bitbucket.org:recafe/membershop-attachment.git
git clone git@bitbucket.org:recafe/membershop-backoffice.git
git clone git@bitbucket.org:recafe/membershop-frontend.git
cd membershop-api
git checkout --track origin/develop
cd ../membershop-backoffice
git checkout --track origin/develop
cd ../membershop-frontend
git checkout --track origin/develop
cd ../
```


## Startup
Go to newmood-docker folder and init env file:
```
cd newmood-docker
cp env-example .env
```

Mac only:
```
docker-sync start
```

```
docker-compose up -d nginx mysql redis elasticsearch
```

Update your machine hosts `/etc/hosts`:
```
# Newmood -------------------------------
127.0.0.1 local.membershop.lt
127.0.0.1 local.membershop.lv
127.0.0.1 local.membershop.ee
127.0.0.1 local.backoffice.membershop.lt
127.0.0.1 local.api.membershop.lt
# ---------------------------------------
```


## Project setup
Login to workspace using `laradock` user:
```
cd newmood-docker
docker-compose exec -u laradock workspace bash
```

### Attachments
Create attachments folder:
```
cd membershop-frontend/public
mkdir attachments
```

Create symlinks to attachments folder:
```
ls -s /var/www/membershop-frontend/public/attachments/ /var/www/membershop-backoffice/public/
ls -s /var/www/membershop-frontend/public/attachments/ /var/www/membershop-api/public/
```

### API
Go to api folder and init env file
```
cd membershop-api
cp .env-example .env
```

Run composer
```
composer install
```

#### Database and Elasticsearch
First login to database, check .env file for data.
`Host: 127.0.0.1`
Insert live data database. _There is a script, which gets data from live, but it needs to be updated to work in docker_

Create elasticsearch index and fill it with products:
```
cd membershop-api
php artisan products:index --startover
php artisan products:index --usebulk
```

### Frontend/Backoffice
Go to frontend/backoffice folder and init env file
```
cd membershop-frontend / cd membershop-backoffice
cp .env-example .env
```

Run composer
```
composer install
```

Install gulp:
```
npm install --global gulp-cli
npm install
gulp
```


Pray!

# Use Docker for local development.

> There are useful docker plugins in Visual Studio code 

## TL;dr for first timers
1. Start the conatiners  using `docker-compose up -d` command
1. Run composer install
1. Import the DB
1. Add info to hosts file

## TL;dr for daily development
1. Run `docker-compose up -d` when starting
1. Run `docker-compose down`
1. Make sure you are in the project Directore

* Firstly install docker and docker compose. Use the below commands if they are fine. (If this does not work refer note *1 below)
```
docker version
docker-compose version
```
* DB creds are in docker-compose.yml check values for MYSQL_ROOT_PASSWORD and others.
* To start the containers.
```
docker-compose up -d
```
* To stop the containers
```
docker-compose down
```
* To connect (ssh) to the php container (If this does not work refer note *2 below)
```
docker exec -it cp-php /bin/bash
```
* To connect (ssh) to the mysql container
```
docker exec -it cp-mysql /bin/bash
```
* You can access phpmyadmin from [http://localhost:8002]
* Add the following to the hosts file. Now instead of using host based URLs now you can use these set of URLs which brings the URL closer to the dev/live environments.
```
127.0.0.1 client.contractorplus.loc
127.0.0.1 cpappapi.contractorplus.loc
127.0.0.1 cpappadmin.contractorplus.loc
127.0.0.1 my.contractorplus.loc
```

## Run composer install
* Connect to the php container
```
docker exec -it cp-php /bin/bash
```
* Navigate to the project folder and run composer install
```
composer install
cd application
composer install
```

## To import DB in your docker conatiner
1. There must be a `cp-vol/data` folder one level up of the project folder.
1. Copy your DB backup eg: DB.sql in the `backup` folder.
1. Connect to your mysql container.
```
docker exec -it cp-mysql /bin/bash
```
1. cd to the data folder in the container.
```
cd /home/data
```
1. Connect to mysql in the container. 
```
mysql -uroot -p contractorplus  (The usual mysql command)
```
1. Source the backup file.
```
source DB.sql
```

## Other commands
If there are any changes to the docker-compose file or the docker files remove the image.
```
docker rmi <image name>
```
* To list the images
```
docker images
```
* Check if the containers are running.
```
docker ps -a
```

* Apache error logs are in ../cp-vol/logs

* Hostname for redis `cp-redis`


Note:
1. If you are on linux based system you need to add sudo before and docker / docker compose commands
1. If your are on windows you need add an extra slash when connect to containers
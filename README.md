# Docker compose cheatsheet #

**Note:** you need to cd first to where your docker-compose.yml file lives.

* Start containers in the background: `docker-compose up -d`
* Start containers on the foreground: `docker-compose up`. You will see a stream of logs for every container running.
* Stop containers: `docker-compose stop`
* Kill containers: `docker-compose kill`
* View container logs: `docker-compose logs`
* Execute command inside of container: `docker-compose exec SERVICE_NAME COMMAND` where `COMMAND` is whatever you want to run. Examples:
  * Shell into the PHP container, `docker-compose exec php-fpm bash`
  * Run symfony console, `docker-compose exec php-fpm bin/console`
  * Open a mysql shell, `docker-compose exec mysql mysql -uroot -pCHOSEN_ROOT_PASSWORD`

# Application file permissions #

As in all server environments, your application needs the correct file permissions to work proberly. You can change the files throught the container, so you won't care if the user exists or has the same ID on your host.

`docker-compose exec php-fpm chown -R www-data:www-data /application/public`

# Recommendations #

It's hard to avoid file permission issues when fiddling about with containers due to the fact that, from your OS point of view, any files created within the container are owned by the process that runs the docker engine (this is usually root). Different OS will also have different problems, for instance you can run stuff in containers using `docker exec -it -u $(id -u):$(id -g) CONTAINER_NAME COMMAND` to force your current user ID into the process, but this will only work if your host OS is Linux, not mac. Follow a couple of simple rules and save yourself a world of hurt.

* Run composer outside of the php container, as doing so would install all your dependencies owned by `root` within your vendor folder.
* Run commands (ie Symfony's console, or Laravel's artisan) straight inside of your container. You can easily open a shell as described above and do your thing from there.

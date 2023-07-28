# Like Service

Contains a RESTful web-service that exposes functionality to like/unlike posts and get the like the like count of a given post.

## Getting started

### Requirements

- You have a MySQL/MariaDB instance running
- You have a Jaeger agent running (see global README)
- You have an instance of the UserAuthService running 

### Running

This service should be run within the unguard kubernetes cluster. (see global README)
If you only want to run the PHP-Server without functionality adjust the ```Dockerfile``` file to only execute  ```php -S 0.0.0.0:8000 -t public``` as the last command.
From within the ```/like-service``` folder you can now run: 

``` bash
docker build .
docker run -p 8000:8000 (image)
```

If you wish to run the PHP-Server outside the kubernetes cluster and with functionality, the requirements must be met and enviroment variables have to bet manually. (not recommended).
Keep in mind that during the startup, the service will create the needed database. 

## Environment variables

To get more information about the JAEGER config options, see https://www.jaegertracing.io/docs/1.19/client-features/

| Name                              | Example Value             | Description                                                  |
|-----------------------------------|---------------------------|--------------------------------------------------------------|
| APP_NAME                          | unguard-like-service      | The name of the application	                               |
| APP_URL		            | http://localhost          | The URL the service will listen to                           |
| UNGUARD_MARIADB_SERVICE_HOST      | 172.0.0.1		        | The host address of the database     		               |
| UNGUARD_MARIADB_SERVICE_PORT_MYSQL| 3306		        | The port of the database                                     |
| DB_USERNAME                       | root                      | The username for the database                                |
| MARIADB_PASSWORD                  | 123456789                 | The password for the database                                |
| UNGUARD_USER_AUTH_SERVICE_ADDRESS | unguard-user-auth-service | The address of the user auth service.                        |
| JAEGER_AGENT_HOST                 | localhost                 | Change to hostname/IP of your Jaeger agent                   |
| JAEGER_SERVICE_NAME               | like-service              | Name that will be used for the service in the Jaeger traces  |
| JAEGER_SAMPLER_TYPE               | const                     | (optional) Set to const to get all traces                    |
| JAEGER_SAMPLER_PARAM              | 1                         | (optional) Set to 1 while sampler is const to get all traces |

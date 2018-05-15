# Docker Composition for Alfresco CE 201707-GA

Sample Docker Composition for integration testing with Alfresco 5.2

**Docker** & **Docker Compose** software is required to use this project.

## Starting

Download or clone this repository.

From root directory, start Docker Compose.

```
$ docker-compose up
```

Once all the containers have been started, a message similar to following one will appear.

```
alfresco_1     | May 15, 2018 11:03:22 AM org.apache.catalina.startup.Catalina start
alfresco_1     | INFO: Server startup in 70314 ms
```

Persistence data folders (`alf_data`, `mariadb_data` & `solr_data`) will be created in the root folder.

```bash
$ tree -L 1
.
├── README.md
├── adf
├── alf_data
├── alfresco
├── docker-compose.yml
├── httpd
├── mariadb_data
├── share
└── solr_data
```

Persistent data folders path can be changed by modifying local paths set in `volumes` directives at `docker-compose.yml` to global paths


## Available services

Once the composition is up, you can check available services:

* Alfresco Repository - http://localhost/alfresco
* Alfresco Share Web App - http://localhost/share
* Alfresco ADF Web App - http://localhost/adf
* SOLR Indexer - http://localhost/solr
* Swagger REST API Doc - http://localhost/api-explorer

## Operations

Following operations are available to customize your Docker Composition.

**Deploying modules**

Copy your artifacts (AMP or JAR) to deployment folders:

* Alfresco Repository
  * `alfresco/assets/amps`
  * `alfresco/assets/jars`

* Share Web App
  * `share/assets/amps_share`
  * `share/assets/jars`


**Configuration**

Modify configuration files:

* Alfresco Repository
  * `alfresco/assets/alfresco/alfresco-global.properties`

* Share Web App
  * `share/assets/share/share-config-custom.xml`


**ADF**

You must add a configuration parameter `--base-href` in your local `package.json` in order to produce an ADF application able to attend `/adf` context path

```
"build": "npm run server-versions && ng build --prod --base-href /adf/",
```

Copy your generated application replacing the content of `adf/assets/app` folder.


## Applying operations

Docker Compose shall be stopped before applying changes.

```bash
$ docker-compose down
```

In order to apply any operation, you need to rebuild Docker image before starting the composition again.

```bash
$ docker-compose build alfresco
$ docker-compose build share
$ docker-compose build adf
```

Once the image has been rebuilt, Docker Compose can be started again.

```
$ docker-compose up
```

## Reseting initial data

In order to remove working data, you can remove all persistent folders.

```bash
$ rm -rf alf_data
$ rm -rf mariadb_data
$ rm -rf solr_data
```

After this operation all the changes in your data will be lost!


# orchestration of services

For the orchestration we use `docker-compose`, to install it on any linux machine:

```
sudo curl -L https://github.com/docker/compose/releases/download/1.17.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

To build all of the images from the `docker-compose.yml`, run: `docker-compose up`.


There should be the following docker images:

* user interface (with potential social media integrations)
* manifesto model with http facade
* news crawler exposing news articles via mongodb.


## build and test single containers

### manifesto data container

add wzb manifesto api key in `persistence/Dockerfile` to environment variable.

```
cd persistence
docker build -t persistence .
docker run persistence
```

this will start the manifesto data download and sqlite3 import.
the data should be accessd via HTTP, e.g:

```
curl "localhost:8888/texts?n=300"
```

will return 300 texts with labels and label sources.


### twitter app

To build and run the twitter-app image:

```
cd apps
docker build -t twitter .
docker run -p 0.0.0.0:80:5000 twitter
```

This will install a `python-3.6` distribution
with all of the requirements and start the web-app
inside the container on port 5000 and forward it to
port 80 on the host machine.

Visit `http://localhost` after you have built and ran the image.

### Installing and running news crawler image

```
cd news_crawler
docker build -f Dockerfile -t crawler .
docker run -p 0.0.0.0:27017:27017 crawler
```
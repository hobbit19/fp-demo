<center><img src="./image/fingerprint.png"></center>

---

# What's this?

This is a server-client app that has the following components:

- __Front end website:__ Gets all information about the user as possible without any dialog, makes a fingerprint and send that information to the backend
- __Backend:__
  - __Nodejs:__ Ensures that the format is correct and stores the data into an elasticsearch database
  - __Elasticsearch:__ Is our database choice
  - __Kibana:__ Provides a front-end to see statistics and queries.

All these components run in it's docker container and we need a __nginx proxy__ because kibana don't support authentication and we don't want to expose the statistics dashboard to the mases.

---

# Running the thing

```bash
  # [optional]You can use your own version of the fp-demo if you build it like this
  docker build fp-demo -t diegoreico/fp-demo
  
  # Then just us docker-compose to create all the containers
  docker-compose up -d
```

 ## Uninstall

 ```bash
  docker-compose down
 ```

---

# Using the thing

## Front-end

You should access to the front-end in the following url:

[http://localhost:1337/node/](http://localhost:1337/node/)

The front-end is not so pretty but it should provide you some info about your browser:

<center><img src="./image/frontend.png"></center>

## dashboard

The URL is:

[http://localhost:1337/kibana/](http://localhost:1337/kibana/)

Default password is __admin__ and user is __admin__

Change by doing:
```bash
htcpasswd -c passwd <username>
```
__Overwrite the file before creating the containers__

![Panel example image](https://github.com/DiegoReiriz/fp-demo/blob/master/image/Captura%20de%20pantalla%20de%202017-05-30%2018-41-30.png?raw=true)

---

# About

## The Schema

The schema of the database is defined in NodeJS using [node-schema-object](https://www.npmjs.com/package/node-schema-object) that lets us ensure that users cannot insert random objects to the database

## The insertion

We'll use the [node elasticsearch connector](https://www.npmjs.com/package/elasticsearch) to help us insert the data into the database.

---

# Todo

1. Delete all of the "trangalladas"
2. Using docker-compose to truly orchestate the thing:
  - fpdemo_node has a slep in order to wait for elasticsearch

---

# Helpful commands

```bash
# Delete dead
docker rm $(docker ps --all -q -f status=dead)
# When a container mountpoint dies
sudo umount /var/lib/docker/devicemapper/mnt/9769dbe9e21d2aba6d2b08e79e3a67534ff8d69b66a93ae554a519cdc9ea7027
rm /etc/nginx/conf.d/default.conf && nano /etc/nginx/conf.d/default.conf
# Delete and re-run containters
docker-compose down && docker-compose up -d --force-recreate
# Reset nginx
apt-get update && apt-get install -y procps
killall nginx && sleep 1 && nginx
```

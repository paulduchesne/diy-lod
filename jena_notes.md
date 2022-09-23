#### notes on deploying jena to ubuntu server

> ssh root@138.197.180.196

> adduser paulduchesne

> usermod -aG sudo paulduchesne

> rsync --archive --chown=paulduchesne:paulduchesne ~/.ssh /home/paulduchesne

> exit

> ssh paulduchesne@138.197.180.196

> sudo apt update

> sudo apt install default-jre

> wget https://dlcdn.apache.org/jena/binaries/apache-jena-fuseki-4.6.1.tar.gz

> tar -xzvf apache-jena-fuseki-4.6.1.tar.gz

> cd apache-jena-fuseki-4.6.1/

> screen ./apache-jena-fuseki-4.6.1/fuseki-server --memTDB /my-dataset

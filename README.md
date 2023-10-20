# CKan-Installation-Guide
# Installing CKAN from package

Welcome to the CKAN (Research Data Management Repository) Installation Guide! This guide will help you set up the environment and install prerequisites on Ubuntu using the terminal.

## Update and Upgrade Ubuntu

`sudo apt-get update`
`sudo apt-get upgrade`

##Install Nginx

Add the Repository

`sudo apt-get install -y software-properties-common`
`sudo add-apt-repository ppa:redislabs/redis`

Install NGINX (a web server) which will proxy the content from one of the WSGI Servers and add a layer of caching:

`sudo apt install -y libpq5 redis-server nginx supervisor`


## Install Python & utilitites

`sudo apt install python3`

`sudo add-apt-repository universe`  `apt-cache search python3.8`  `sudo add-apt-repository ppa:deadsnakes/ppa`

`sudo apt-get install python3.8-distutils`

## Download the CKAN package

### On Ubuntu 20.04:

`wget https://packaging.ckan.org/python-ckan_2.10-focal_amd64.deb`

### On Ubuntu 22.04

`wget https://packaging.ckan.org/python-ckan_2.10-jammy_amd64.deb`

## Install the CKAN package

### On Ubuntu 20.04

`sudo dpkg -i python-ckan_2.10-focal_amd64.deb`

## On Ubuntu 22.04

`sudo dpkg -i python-ckan_2.10-jammy_amd64.deb`

## Install and configure PostgreSQL

### Install PostgreSQL required packages

`sudo apt install -y postgresql`

## Check that PostgreSQL was installed correctly

`sudo -u postgres psql -l`

## Create a new PostgreSQL user for CKAN

Create a new PostgreSQL user called ckan_default, and enter a password for the user when prompted. You’ll need this password later

`sudo -u postgres createuser -S -D -R -P ckan_default`

## Create a new PostgreSQL database

Create a new PostgreSQL database, called ckan_default, owned by the database user you just created

`sudo -u postgres createdb -O ckan_default ckan_default -E utf-8`

## Setup CKAN URL

`Open CKAN configuration file: ckan.ini`

sudo nano  /etc/ckan/default/ckan.ini

Find "Site Settings" in the file and insert your URL

    ckan.site_url =http://your_url.com

Find "Search Settings" and insert same url

    ckan.site_id = http://your_url.com

Save and exit.

## Installation and customisation of SOLR

`sudo apt install -y solr-tomcat`

`sudo mv /etc/solr/conf/schema.xml /etc/solr/conf/schema.xml.bak`

`sudo ln -s /usr/lib/ckan/default/src/ckan/ckan/config/solr/schema.xml`

`sudo ln -s /usr/lib/ckan/default/src/ckan/ckan/config/solr/schema.xml /etc/solr/conf/schema.xml`

`sudo service tomcat9 restart`

`sudo nano /etc/ckan/default/ckan.ini`

Find "Search Settings" and insert solr url

    solr_url = http://your_url.com/solr

`sudo ckan db init`

`sudo supervisorctl reload`

`sudo supervisorctl status`

`sudo service nginx restart`

## CKAN installation is completed. Now you have to assign admin email id, username and password.

Replace username and email id with your email id and desired username.

`sudo ckan -c /etc/ckan/default/ckan.ini sysadmin add username email=youremail@institute.com name=username`

    Click on 'y'. Enter your password.

## Now search your your URL in browser and user same username and password to access to admin panel of the CKAN.



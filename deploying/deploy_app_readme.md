# Deploying starter app
- [Deploying starter app](#deploying-starter-app)
  - [App Script](#app-script)
    - [Different ways to run apps](#different-ways-to-run-apps)
    - [Errors](#errors)
- [Deploying database](#deploying-database)
  - [DB Script](#db-script)

This markdown entails the script to deploying and app that is linked to a specific database.

**Note**
Run manually first then paste full script, 
enable nginx when you start your instance nginx will start too, to test new scripts you have to create a new instance, needs port 3000 as sg

## App Script
```bash  
#!/bin/bash

# Commands to remove user input
# DEBIAN_FRONTEND=noninteractive eg sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y or sudo sed -i "s/#\$nrconf{kernelhints} = -1;/\$nrconf{kernelhints} = -1;/g" /etc/needrestart/needrestart.conf


echo update
# updates the information about available packages without installing
sudo apt update -y
echo done!

echo upgrade
# installs updates
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo done!

echo install nginx
# nginx will be our web server the primary role of a web server is to store, process, and deliver requested information or webpages to end users
sudo DEBIAN_FRONTEND=noninteractive apt install nginx -y
echo done!

# configure reverse proxy
# sits behind the firewall in a private network and directs client requests to the appropriate backend server
# cd /etc/nginx/sites-available/default try_files $uri $uri/ =404; ---> proxy_pass http://localhost:3000;

# changing config file
sudo sed -i '51s/.*/\t        proxy_pass http:\/\/localhost:3000;/' /etc/nginx/sites-available/default

echo restart nginx
# activates any changes made during configuration
sudo DEBIAN_FRONTEND=noninteractive systemctl restart nginx
echo done!

echo enable nginx
# allow any user logging in to use nginx
sudo DEBIAN_FRONTEND=noninteractive systemctl enable nginx
echo done!

# get the app folder with app code inside
# scp -i ~/.ssh/tech258.pem -r ~/app-code/sparta_test_app ubuntu@ec2-63-35-215-215.eu-west-1.compute.amazonaws.com:~

# or

echo get app folder
# git clone from remote repo for app that will be run
sudo DEBIAN_FRONTEND=noninteractive git clone https://github.com/temianibaba/tech258-sparta-test-app.git
echo done!



# cd app folder
echo go to app folder
cd /home/ubuntu/tech258-sparta-test-app/app
echo done!

# run app
echo install node js
sudo DEBIAN_FRONTEND=noninteractive curl -fsSL https://deb.nodesource.com/setup_20.x | sudo DEBIAN_FRONTEND=noninteractive -E bash - &&\

sudo DEBIAN_FRONTEND=noninteractive apt-get install -y nodejs

node -v # to check version
echo done!

# set DB_HOST env var (npm install is looking for this, it will continue without one and it won't work) also get private IP address
# export DB_HOST=mongodb://(db private IP):27017/posts

export DB_HOST=mongodb://172.31.53.85:27017/posts
printenv DB_HOST

echo install app
# install app to instance
sudo -E npm install
echo done!

echo start app
sudo -E npm start
```

**note:** to check app is running use your IP address with **:3000**

### Different ways to run apps 
1. ``sudo npm start or node app.js``
2. ``nohup node app.js &`` - runs app in background
3. ``pm2`` - runs app in background as parent process
```bash
sudo npm install pm2 -g
echo stop all processes
sudo pm2 stop all
echo done!

echo start app in background
pm2 start app.js # (name process)
echo done!
```

### Errors
Sometime nginx doesn't work after changing config. If you need to reinstall nginx use:
```bash
sudo systemctl stop nginx

sudo apt purge nginx nginx-common
```

# Deploying database

**note:**/posts in url to check if database is up and running<br>
**note:** npm install will populate with dummy data into data base (clears and reseeds)

1. New VM
tech258_muyis_first_deploy_db
same image
same key
sg- SSH Anywhere, Custom TCP port number:27017 anywhere

2. SSH into new VM

3. Plan commands 
## DB Script
```bash
nano provision.db
#!/bin/bash

echo update
sudo apt update -y
echo done!

echo upgrade
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo done!

# install mongo db 7.0.6
sudo DEBIAN_FRONTEND=noninteractive apt-get install gnupg curl

# To import the MongoDB public GPG key,
curl DEBIAN_FRONTEND=noninteractive -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor

# Create the list file /etc/apt/sources.list.d/mongodb-org-7.0.list for your version of Ubuntu.
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

sudo apt-get update

# To install a specific release, you must specify each component package individually along with the version number, as in the following example mongosh has to be 2.2.4
sudo DEBIAN_FRONTEND=noninteractive apt-get install -y mongodb-org=7.0.6 mongodb-org-database=7.0.6 mongodb-org-server=7.0.6 mongodb-mongosh=2.2.4 mongodb-org-mongos=7.0.6 mongodb-org-tools=7.0.6

#checks
mongod --version
sudo systemctl status mongod

# config bind IP in mongo db config file - 0.0.0.0 (where will mongo db allow connections from)
# 127.0.0.1 ---> 0.0.0.0
# manually sudo nano /etc/mongod.conf  change bindIP
# sudo sed -i 's/^\(\s*\)bindIp: .*/\1bindIp: 0.0.0.0/' /etc/mongod.conf
sudo sed -i -r 's/(\b[0-9]{1,3}\.){3}[0-9]{1,3}\b'/"0.0.0.0"/ /etc/mongod.conf



# restart mongo db 
sudo systemctl restart mongod

# enable mongo db
sudo systemctl enable mongod
```
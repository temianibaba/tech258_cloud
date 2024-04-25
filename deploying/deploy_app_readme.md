# Deploying starter app
- [Deploying starter app](#deploying-starter-app)
  - [App Script](#app-script)
    - [Explanation](#explanation)
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

echo update
sudo apt update -y
echo done!

echo upgrade
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo done!

echo install nginx
sudo DEBIAN_FRONTEND=noninteractive apt install nginx -y
echo done!

# changing config file
sudo sed -i '51s/.*/\t        proxy_pass http:\/\/localhost:3000;/' /etc/nginx/sites-available/default

echo restart nginx
sudo DEBIAN_FRONTEND=noninteractive systemctl restart nginx
echo done!

echo enable nginx
sudo DEBIAN_FRONTEND=noninteractive systemctl enable nginx
echo done!

echo get app folder
sudo DEBIAN_FRONTEND=noninteractive git clone https://github.com/temianibaba/tech258-sparta-test-app.git
echo done!

echo go to app folder
cd ~/tech258-sparta-test-app/app
echo done!

echo install node js
sudo DEBIAN_FRONTEND=noninteractive curl -fsSL https://deb.nodesource.com/setup_20.x | sudo DEBIAN_FRONTEND=noninteractive -E bash - &&\

sudo DEBIAN_FRONTEND=noninteractive apt-get install -y nodejs

node -v # to check version
echo done!

# export DB_HOST=mongodb://(db private IP):27017/posts
export DB_HOST=mongodb://10.0.3.4:27017/posts
printenv DB_HOST

echo install app
sudo -E npm install
echo done!

echo start app
sudo -E npm start
```

**note:** to check app is running use your IP address with **:3000**
### Explanation
- **Commands to remove user input** DEBIAN_FRONTEND=noninteractive eg: `sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y` or `sudo sed -i "s/#\$nrconf{kernelhints} = -1;/\$nrconf{kernelhints} = -1;/g" /etc/needrestart/needrestart.conf`
- **Updates and upgrades** updates the information about available packages without installing, upgrade installs the updates which can lead to incompatability and can damage the instance.
- **NIGINX** will be our web server the primary role of a web server is to store, process, and deliver requested information or webpages to end users.
- **Reverse proxy** sits behind the firewall in a private network and directs client requests to the appropriate backend server. `cd /etc/nginx/sites-available/default` try_files $uri $uri/ =404; ---> proxy_pass http://localhost:3000;
- **Restart and enable**  activates any changes made during configuration, allow any user logging in to use nginx.
- **Getting app into instace** you can use `scp` command ``scp -i ~/.ssh/tech258.pem -r ~/app-code/sparta_test_app ubuntu@ec2-63-35-215-215.eu-west-1.compute.amazonaws.com:~`` **OR** `git clone` (which is faster) your repo. 
- **Intalling node js** node js helps give commands to install and run the app
- **Making an environment variable** using the `npm install` command when installing your app, a `DB_HOST` env variable is needed to link the app to the database, if deploying the app alone (a monolith architecture) then you don't have to worry about this. 

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

# If ran on an environment with mongodb installed already we need to remove it first
# sudo rm -f /usr/share/keyrings/mongofb-server-7.0.gpg

# To import the MongoDB public GPG key,
curl DEBIAN_FRONTEND=noninteractive -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor # --yes

# Create the list file /etc/apt/sources.list.d/mongodb-org-7.0.list for your version of Ubuntu.
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

sudo apt-get update

# To install a specific release, you must specify each component package individually along with the version number, as in the following example mongosh has to be 2.2.4
sudo DEBIAN_FRONTEND=noninteractive apt-get install -y mongodb-org=7.0.6 mongodb-org-database=7.0.6 mongodb-org-server=7.0.6 mongodb-mongosh=2.2.4 mongodb-org-mongos=7.0.6 mongodb-org-tools=7.0.6

#checks
mongod --version
sudo systemctl status mongod

# config bind IP in mongo db 
sudo sed -i -r 's/(\b[0-9]{1,3}\.){3}[0-9]{1,3}\b'/"0.0.0.0"/ /etc/mongod.conf

# restart mongo db 
sudo systemctl restart mongod

# enable mongo db
sudo systemctl enable mongod
```

**Configuration of mongo db** config file - 0.0.0.0 (where will mongo db allow connections from), manually ``sudo nano /etc/mongod.conf``  change bindIP from 127.0.0.1 to 0.0.0.0
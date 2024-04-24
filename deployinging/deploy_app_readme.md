# Deploying starter app



**Note**
Run manually first then paste full script, 
enable nginx when you start your instance nginx will start too, to test new scripts you have to create a new instance

## Script
```bash  
#!/bin/bash

# Commands to remove user input
# DEBIAN_FRONTEND=noninteractive eg sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y or sudo sed -i "s/#\$nrconf{kernelhints} = -1;/\$nrconf{kernelhints} = -1;/g" /etc/needrestart/needrestart.conf

echo update
sudo apt update -y
echo done!

echo upgrade
# fix this command! asks for user input
# pending kernel upgrade
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo done!

echo install nginx
# fix this command! asks for user input
# pending kernel upgrade
sudo DEBIAN_FRONTEND=noninteractive apt install nginx -y
echo done!

# configure reverse proxy

# changing config file

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
sudo DEBIAN_FRONTEND=noninteractive git clone https://github.com/temianibaba/tech258-sparta-test-app.git
echo done!

# cd app folder
echo go to app folder
cd /home/ubuntu/tech258-sparta-test-app/app
echo done!

# set DB_HOST env var (npm install is looking for this, it will continue without one and it won't work) also get private IP address
# export DB_HOST=mongodb://(db private IP):27017/posts
export DB_HOST=mongodb://172.31.44.25:27017/posts

# run app
echo install node js
sudo DEBIAN_FRONTEND=noninteractive curl -fsSL https://deb.nodesource.com/setup_20.x | sudo DEBIAN_FRONTEND=noninteractive -E bash - &&\

sudo DEBIAN_FRONTEND=noninteractive apt-get install -y nodejs

node -v # to check version
echo done!


echo install app
sudo -E npm install
echo done!

sudo npm install pm2 -g
echo stop all processes
sudo -E pm2 stop all
echo done!

echo start app in background
pm2 start app.js # (name process)
echo done!
```

**note:** to check app is running use your IP address with **:3000**

### Different ways to run apps 
1. ``sudo npm start or node app.js``
2. ``nohup node app.js &``
3. ``pm2``
```bash
sudo npm install pm2 -g
echo stop all processes
sudo pm2 stop all
echo done!

echo start app in background
pm2 start app.js # (name process)
echo done!
```
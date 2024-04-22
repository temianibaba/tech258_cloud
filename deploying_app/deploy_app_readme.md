# Deploying starter app



**Note**
Run manually first then paste full script, 
enable nginx when you start your instance nginx will start too, to test new scripts you have to create a new instance

### Final Script
```  
#!/bin/bash

# Commands to remove user input
# DEBIAN_FRONTEND=noninteractive eg sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y or sudo sed -i "s/#\$nrconf{kernelhints} = -1;/\$nrconf{kernelhints} = -1;/g" /etc/needrestart/needrestart.conf

echo update
sudo apt update -y
 echo done!

echo upgrade
# fix this command! asks for user input
# pending kernek upgrade
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
sudo apt upgrade -y
 echo done! 

echo install nginx
# fix this command! asks for user input
# pending kernek upgrade
sudo DEBIAN_FRONTEND=noninteractive apt install nginx -y
sudo apt install nginx -y
 echo done!

# configure reverse proxy

# changing config file

echo restart nginx
sudo systemctl restart nginx
 echo done!

echo enable nginx
sudo systemctl enable nginx
echo done!

# get the app folder with app code inside
scp -i ~/.ssh/tech258.pem -r ~/app-code/sparta_test_app ubuntu@ec2-63-35-215-215.eu-west-1.compute.amazonaws.com:~

or

git clone https://github.com/temianibaba/tech258-sparta-test-app.git

# cd app folder
cd app/

# run app
npm install
npm start or node app.js
```
**note:** to check app is running use your IP address with **:3000**

### Running app in the background
`` nohup node app.js &
``

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

curl DEBIAN_FRONTEND=noninteractive -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor -y

echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

sudo apt-get update

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
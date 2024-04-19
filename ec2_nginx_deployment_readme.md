# Deploying a VM SBS

## 1. Login to AWS and click on services
![image.png](images%2Fimage.png)<br>
## 2. Go to the compute section and click on EC2 (elastic compute cloud)
![img.png](images%2Fimg.png)<br>
## 3. Click on launch instance
![img_1.png](images%2Fimg_1.png)<br>
## 4. Make sure to put some thought into the name of your instance 
![img_2.png](images%2Fimg_2.png)<br>
## 5. Choose your machine image
![img_3.png](images%2Fimg_3.png)<br>
## 6. Choose your instance type and key pair login
![img_4.png](images%2Fimg_4.png)<br>
## 7. Choose key pair login
![img_5.png](images%2Fimg_5.png)<br>
## 8. Click on edit in network settings
![img_6.png](images%2Fimg_6.png)
## 9. Add HTTP security group
![img_7.png](images%2Fimg_7.png)
## 10. Click on launch instance
![img_8.png](images%2Fimg_8.png)
## 11. Click on your instance ID
![img_9.png](images%2Fimg_9.png)
## 12. Click on your instance ID
![img_10.png](images%2Fimg_10.png)
## 13. Click on connect and go to SSH client
![img_11.png](images%2Fimg_11.png)
## 14. AWS will give you commands to copy tou your terminal
![img_12.png](images%2Fimg_12.png)
## 15. In your terminal navigate to your SSH key folder
![img_13.png](images%2Fimg_13.png)
## 16. Execute the `chmod 400 "tech258.pem"` command
![img_14.png](images%2Fimg_14.png)
## 17. Execute the `ssh -i "tech258.pem" ubuntu@ec2-18-203-93-103.eu-west-1.compute.amazonaws.com` command
![img_15.png](images%2Fimg_15.png)
## 18. Say yes so you can automatically connect
![img_16.png](images%2Fimg_16.png)
![img_17.png](images%2Fimg_17.png)
## Now you are connected to your instance
** **
# Installing nginx

## 1. Execute `sudo apt update -y`
![img_18.png](images%2Fimg_18.png)
## 2. Execute `sudo apt upgrade -y`
![img_19.png](images%2Fimg_19.png)
## 3. Execute `sudo apt  install nginx -y`
![img_20.png](images%2Fimg_20.png)
## After this you have installed nginx but if you would like to check use `systemctl status nginx`
![img_21.png](images%2Fimg_21.png)
** **
Command list
```commandline
chmod 400 "tech258.pem"
ssh -i "tech258.pem" ubuntu@ec2-18-203-93-103.eu-west-1.compute.amazonaws.com
sudo apt update -y
sudo apt upgrade -y
sudo apt  install nginx -y
systemctl status nginx
```
To access your instance via your browser you would copy your Public IPv4 address into where you type out URLs

** **
# Explanation
## Machine images
A machine image is a Compute Engine resource that stores all the configuration, metadata, permissions, and data from multiple disks of a virtual machine (VM) instance. You can use a machine image in many system maintenance, backup and recovery, and instance cloning scenarios. The machine image (in Amazon, the AMI) is a raw copy of your operating system and core software for a particular environment on a specific platform. When you start a virtual server, it copies its operating environment from the machine image and boots up.
## Security groups
A security group controls the traffic that is allowed to reach and leave the instance that it is associated with. It is like a firewall, it's the most basic form of security group. For example, after you associate a security group with an EC2 instance, it controls the inbound and outbound traffic for the instance.
**Ports** are door numbers inside your instances IP, if your IP is like an address the port is the door number and when attached to a security rule you are allowing traffic through that door. For example the port number for SSH is 22, once this rule is allowed anyone with the corresponding SSH key has access to your instance. 
## SSH keys
SSH keys encrypt data transfer and only allow users with the corresponding key to decrypt the data received or sent. In this case we have access to the shell, the instance, because we have set up the SSH key pair.
## Command list
```commandline
chmod 400 "tech258.pem"
ssh -i "tech258.pem" ubuntu@ec2-18-203-93-103.eu-west-1.compute.amazonaws.com
sudo apt update -y
sudo apt upgrade -y
sudo apt  install nginx -y
systemctl status nginx
```
`chmod 400`: change mode<br>
`sudo`: 'superuser do' this is a command allows you to run programs with the security privileges.<br>
`apt`: You can use the apt command to install, delete or remove apps, keep Ubuntu/Debian server up to date with security patches and more.<br>
`update`: this command checks the system has all the latest packages available. <br>
`upgrade`: this command the makes all the packages installed are at their latest version. <br>
`-y`: this command tells the system to choose yes for any questions we may be asked during the running of the process, using this command allows you to automate the process.<br>
## NGINX
NGINX is open source software for web serving, reverse proxying, caching, load balancing, media streaming, and more. 
## Checks
There are two ways to check:<br>
1. Use `systemctl status nginx`
![img_21.png](images%2Fimg_21.png)
2. Enter your IP address into your web browser

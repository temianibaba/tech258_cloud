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

## AWS Cloud: Add users to EC2 Ubuntu instance

If you already have access to your Amazon EC2 Ubuntu instance and are wondering how to add new users with SSH access, follow these steps:

### SSH into your own instance

Let’s assume your IPV4 address is ec2–393–34–23–6759.us-east-2.compute.amazonaws.com and default key name is default-pem.pem


```
ssh -i default-pem.pem ubuntu@ec2–393–34–23–6759.us-east-21.compute.amazonaws.com
```


### Add a new user


```
sudo adduser desaijay
```


If you want to add the user to the admin group:


```
sudo adduser desaijay admin
```


If you want to avoid typing the password over and over:


```
sudo vi /etc/sudoers
```


Add this below the user privilege section:


```
desaijay ALL=(ALL) NOPASSWD:ALL
```


### Switch the terminal session with a new user account


```
sudo su desaijay
```


### Create a directory named **.ssh** in the home folder:


```
chmod 700 .ssh
```


### Create authorized_keys file inside .ssh directory


```
chmod 600 authorized_keys
```


### Generate the public key

Open the new tab in the terminal and hit the command to generate the RSA key-pair:


```
ssh-keygen -t rsa
```


Copy the key-pair:


```
cat ~/.ssh/id_rsa.pub | pbcopy
```


Now go to the previous tab in the terminal and paste the key inside the **authorized_keys** file


```
ssh-rsa AAAAB3NzaC1yc2EApYM+jrV08qOf3KyoOsHS/JFPf2Hcmv1FPnz3fs7TODIcgt5eC07d9FOu2PSXa1Pklasf9NZMHArP9woatVXwlx0EpGX1ofnIsvU/2/r7VUFqfM2Ms3ahioeH4iyceA8z+PHxSzVjD4FanB9Bqqp6bYoztNoMtmfksl7XMDxm6d+PbA4pChQTFHZN+gjED8Hl9SClQuDdZCM6mBcFWnNhoRWkNZHujJqxE25yl4Zc2KaRLPAPJRpnwu6uDsUIfNuTdqPui
```


Now, exit the ec2 instance. We will try to log in with our new user **desaijay:**


```
ssh desaijay@ec2–393–34–23–6759.us-east-21.compute.amazonaws.com
```


After hitting this command you may see that we are now logged in with our new user.

### Conclusion

In this short article, we saw how to create a new user in an Amazon EC2 Ubuntu instance.

Please feel free to reach to me for any queries in the comment section.
## DevOps 101 - Deploying & Configuring Jenkins on AWS server


![devops image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601637194676/LME_wBajG.png)

Hello!

In this series, we are going to learn how we can enable the entire devops CI/CD pipeline for a custom application that can be deployed to the production. The goal of this series to deploy our Next.js application on Kubernetes.

Our CI/CD pipeline build in our DevOps Bootcamp series can be used across all the NodeJs, Angular, React.Js and Vue.Js projects. It will enable a very minimal setup time with maximum output for the developers who want to spend more time working on the application features. 

This is a hands-on guide featuring all the tools we would be using in our continuous integration and continuous deployment process, hence my recommendation is to practice, run commands while you go through the articles.

**Let’s discuss some of the pros of continuous integration and continuous deployment.**


- Faster Release Cycle
- Fewer Bugs
- Less risk on the release
- More Testing Reliability
- Easy to maintain and update the software
- Helps developers to focus more on building the product features better and faster


### Tools used

- Jenkins
- Ansible
- Docker
- Kubernetes
-  [Node](https://nodejs.org/en/) 
-  [Next.Js](https://nextjs.org/) 

### Pre-Requisites

1. Create an AWS account - https://aws.amazon.com
2. Little knowledge of Next.js and Node.js would be great, but not necessary for our setup. CodeBase for the entire setup -  [https://github.com/desaijay315/kubernetes-with-nextjs-app](Link) 

In this part, we would be focusing on installing and deploying Jenkins on the AWS server as our first step towards CI/CD pipeline.


**Create an EC2 instance**

- Login to your AWS account
- Go to services and select EC2 Instance
- Choose an Amazon Linux Image. (You can also choose any other image as per your requirement)

![Choose an amazon Linux image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601442432505/AlD_TMci6.png)

- Choose an Instance Type

![step 2 - choose and instance type.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601442590586/tJaOwJbR7.png)

- Configure an Instance Type 

Make sure you enable the auto-assign public IP or you will have to manually assign the elastic IP for your instance. It was disabled in my case.


![Steps 3 - configure instance details.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601442600971/xKyS7jXfy.png)


- Add Tags

![step 4  - add tags .png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601443145961/W2oHOJqog.png)

-** Configure the security group **- As Jenkins runs on port 8080 by default, We need to enable to inbound traffic to port 8080. Secondly, add a rule for the SSH port 22, to access our server. To create a security group, refer below link -

 [https://djaytechdiary.com/creating-security-groups-in-aws-ckfpdurto002vxus1bxvmah7e
](Link) 


![step 5 - security group.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601443185288/VLWJQKK5E.png)

- Review instance and launch 

![review and launch.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601443224253/9IK7DyEeQ.png)

- Create a key, if you don’t have the existing keys with you, create a new one, store it on the machine and launch the instance. Most importantly, do not lose them or else you will lose your server access!

![step 5 - security group.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601443318820/DLXHZ3KPo.png)

### HOORAY!! Server launch is successful

![launch status.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601443383559/2JeqYZhMw.png)

- Go to the EC2 instance and confirm the instance status. It should be up and running. Status check - 2/2
![jenkins_server_instande.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601443904016/zXjuEo7sj.png)

## Installing Jenkins

 - Log in to ec2 instance with public IP - ssh -i jenkins_aws_key.pem ec2-user@your-server-public-IP
- Switch to the root user 
```
sudo su -
``` 
- By default our amazon image has java1.7 version, we would upgrade it to java1.8.


%[https://gist.github.com/desaijay315/a908b2c97c89454363f761938ad2f24d
]

 - Vola!! Jenkins Unlocked!

![jenkins up.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601444354368/sYLUocvl3.png)

- Get the Admin password from the server - 

```
cat /var/lib/jenkins/secrets/initialAdminPassword

``` 
- We will skip the installation of the plugins for now and install the plugin as and when  require to set up our code build process

![skip the install plugins.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601444467725/c1R2fmZji.png)

 - Welcome to Jenkins

![welcome to jenkins.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601444533146/bmKpmipV5.png)

- ** To change the port from 8080 to 9090** - By default we have our Jenkins up and running on the port 8080. Let’s change it to 9090. We need to add an inbound rule for port 9090 in the security group which we created.

- Open then Jenkins configurations

```
vi /etc/sysconfig/jenkins
```
- The only part you need to change is:

```
JENKINS_PORT="8080"
```

- There you need to change to the desired port. For example:

```
JENKINS_PORT="9090"
```

- Finally, Restart Jenkins service

```
service Jenkins restart

```
 - You can now see the port is changed to 9090


![port change.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601444752954/68Qs0s9Ov.png)

### Installing GIT on Jenkins Server

We will integrate GIT with Jenkins, and enable them in our Jenkins job, this will help Jenkins to pull our code and build the latest code commits.

Open your terminal and run 

```
 yum install git -y
```

### Installing GIT Plugins Jenkins Server

- Install Github from the manage plugins section. Go to the available section and install **“Github”**


![manage plugins jenkins -1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601444900709/hg7THnWwk.png)

- Now go to global tool configuration 

Change the name value from **Default to GitHub and uncheck install automatically box**

![github global tool configuration.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601445010548/t8047I0H6.png)

- You’re ready with the Jenkins setup and WE'RE DONE!


**Conclusion**

In this article, we installed and deployed Jenkins on our AWS server. We learned how different plugins can be configured and installed, and how the default port of our Jenkins server can be changed.

In the  [next article](https://djaytechdiary.com/dockerize-your-next-js-application), we will configure our NextJs app and dockerize the application.


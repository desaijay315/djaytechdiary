## What are GitHub Actions and How can we automate application deployments using GitHub Actions?

Nowadays, developing software without a CI/CD pipelines and without a DevOps process is unimaginable. Your version control is tied up to the operational process so that the piece of the software is deployed on the production servers. Understanding DevOps really involves understanding these common problems and recognising solutions to them.

The common theme percolates through these solutions is the bringing together of the traditionally separate worlds of Development and Operations.

**What is covered in our article?**

We will learn what are GitHub actions, deploy our Node.js application using Github actions and creating a complete CI/CD pipeline workflow and 

many more....

**Pre-requisites**

- Install Nodejs and npm on your workstation
- Create an AWS Cloud account

### What are Github Actions?

The concept of using tools for CI/CD isn't new. Many companies and teams leverage Jenkins, CircleCI and Travis CI to accomplish deployment workflow and automate things by running scripts in CI job.

I have already used a bit of Jenkins as a continuous integration server and wanted to try Github Actions. The idea here is to automate the entire CI/CD pipeline using Github Actions if you are storing your code on Github and deploy our application on the remote server. 

Github Actions allow creating a complex workflow using automated tasks (called actions) in order to be activated and run when a commit is done in your Github repository.  You can choose which kind of operating system will be used to run actions: Ubuntu Linux, Windows or Mac OS X.

### Advantages of using Github Actions Workflow

- The individual actions in a workflow are isolated by default. You can use a completely different computing environment for, say, compilation and testing.
- There are already open-source reimplementations of GitHub actions, such as an action for local testing.
- Ready actions in the GitHub marketplace.
- No need to set up webhooks and access tokens for connecting your CI/CD service to GitHub — because GitHub Actions is already in GitHub!
- Configs will always be stored in .github


*So, let us go ahead and execute below commands, write some code and automate deployments by leveraging Github Actions.*

- Create the project folder 

```
mkdir nodejs-github-actions
npm init
```

- Create a package.json file into the root of our project folder nodejs-github-actions

*If you want to skip all the question asked during the creation of package.json with the above command, run:*

```
npm init -y
```

-  Install the latest version of packages needed to develop our application

```
npm install express --save
```

### Express Js

*Express.js is a minimal and flexible Nodejs framework which provides lots of features to develop web and mobile applications. It's easy to create an API with HTTP utility and middlewares with Express.js*

- Create a server.js file into the src of our project folder *nodejs-github-actions*

```
mkdir src
touch src/server.js
```

- Insert the below code

```js
const express     = require('express')
const app         = express();
const port        =  process.env.PORT || 3000

app.listen(port,() =>{
    console.log('server is up on ' + port);
})
```

- Create the routes folder inside the src folder. We would be creating all our routes here.

```
mkdir src/routes
touch src/routes/index.js
```

- Create some routes for our demo project. Insert the below code inside routes/index.js

```js
const express = require("express");
const router = new express.Router();

router.get("/", (req, res) => {
  res.status(200).json({ message: "hello" });
});

router.get("/dashboard", (req, res) => {
  res.status(200).json({ message: "this is a dashboard" });
});

router.get("/user/profile", (req, res) => {
  res.status(200).json({
    name: "john doe",
    age: "30",
    location: "Mumbai",
  });
});

module.exports = router;
```

- Now update the server.js file to include routes

```js
const express = require("express");
const indexRoutes = require("./routes/index");

const app = express();
const port = process.env.PORT || 3000;

app.use(express.json());
app.use("/api", indexRoutes);

app.listen(port, () => {
  console.log("server  is up on " + port);
});
```

- Check if our application is working fine

```
npm run start
```

![npm_start.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603030949382/hrgiHYPHD.png)

### Push our code to GitHub 

- Create .gitignore file to exclude node_modules from getting pushed to our git repo

```sh
touch .gitignore
vim .gitignore

#add below line
node_modules
```

- Now, let's push our code to GitHub.

```sh
#initialize git
git init

#Add all the files
git add .

#commit
git commit -m "nodes GitHub actions"

#set the remote repository if you have not set it up
git remote add origin https://github.com/desaijay315/nodejs-github-actions.git

#push the code
 git push origin master
```

**- Let's go to Github Actions
**

![image_1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603015105055/0Xpd2YmBK.png)

- Create the Nodejs Workflow.

![image_2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603015180178/H9zBpeech.png)

- Click on start commit and commit the file to the master branch. This will create the workflow file for nodejs application.

![commit_4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603016578226/HP79WjIan.png)

- Take a *git pull* on the project folder.  This will fetch the */.github/workflows/node.js.yml* file which was just created in the workflow step

```
git pull
```

- Insert the below code inside node.js.yml file. 

```yml
name: Node.js CI

on:
  push:
    branches: [master]

jobs:
  build:
    runs-on: self-hosted

    strategy:
      matrix:
        node-version: [14.x]

    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm i
```

### Github Action Node.js Workflow

**name** - This is the workflow name appears by default and this is *optional*.

**on** - This event triggers the workflow file to run jobs. In our example, we are using push event, so on every push made to the master branch of our repository, the job will run build will be generated. We can set up the workflow to only run on certain branches, paths, or tags.

**jobs** -  Workflows are made up of one or more jobs that can be run on your repository and all the jobs in the GitHub actions will run parallelly by default. 

**runs-on ** - The type of machine we would like to run on. We would be using self-hosted runner as we are using an operating system which might not be supported by Github hosted runner. We would also self-hosted hardware configurations on the AWS cloud.

**matrix** - We would be using matrix to specify which programming language and version will be supported by our Github actions workflow. It also supports the different operating system and we can create a variety of configurations and multiple jobs

**steps**  - Groups all the step to run on our self hosted runner

**uses: actions/checkout@v2** -  This will checkout the Github code from our repo and pushes the code on our runner to perform further steps/actions on our codebase.

**uses: actions/setup-node@v1** - This will install npm on the runner. It won't install npm if it's already cached on the servers.

** run: npm i** - the run command executes the job on the runner. Here we would be installing all the packages of our project.

- Let's go to the Github settings and click on the actions

![action_settings_1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603020200259/LXLKcAMoI.png)

- Enable all the actions

![action_settings_nodejs_3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603020224829/wAK4TiJIo.png)

- Add the runner

![add_runner_1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603020290899/BOJnHQZ8s.png)

- We can select the operating system and the architecture accordingly. We would go with Linux and ARM64.

![self_hosted_4_runner_4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603020508144/YWElYYjBh.png)

- Run the job and we can see that this build will fail currently, as we have not set up our cloud environment to deploy our application and our runners are not connected with the remote host. Let's connect our GitHub actions with our productions servers.

*We would be deploying our application on  [AWS cloud environment](https://console.aws.amazon.com/console/home), let's create the EC2 instance and configure it ready for our deployments.*

- Create an EC2 instance. 

*We would select the Ubuntu operating system. Visit  [this](https://djaytechdiary.com/install-and-configure-jenkins-on-amazon-ami) article, if you want to learn more about how to create an EC2 instance.*

![ubuntu_5.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603020962655/trO7psNpg.png)

- Review our launch of the EC2 instance

![review_launch_7.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603021014954/-uJaQclg9.png)

-  [Create security groups](https://djaytechdiary.com/aws-security-groups). Make sure the inbound port 80 & 443 is enabled.

![security groups.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603021126656/OHfLZITqV.png)

Now, let's login to our server to perform activities to get our runner up and running and to connect with GitHub actions self-hosted runner.

```sh
ssh -i jenkins_aws_key.pem ubuntu@your-server-public-IP

#login to root user
sudo su - 
```

- Create a new user providing the root privileges

```sh
adduser desaijay #this will ask you to set a password and certain questions such as 

#Enter the new value or press ENTER for the default
#Full Name []:
#Room Number []:
#Work Phone []:
#Home Phone []:
#Other []:

usermod -aG root desaijay
```
Log in with the user **desaijay**

```sh
su - desaijay
```

Now let's run all the below commands to configure and execute the GitHub actions runner, so that our self-hosted runner and listen to our GitHub events/actions/commands. You can get all the commands for your setup on your repository.  [Click here to get below commands](https://github.com/desaijay315/nodejs-github-actions/settings/actions/add-new-runner?arch=arm64&os=linux) 

**Download**

```sh
#Create a folder
mkdir actions-runner && cd actions-runner

#Download the latest runner package
curl -O -L https://github.com/actions/runner/releases/download/v2.273.5/actions-runner-linux-arm64-2.273.5.tar.gz

#Extract the installer
tar xzf ./actions-runner-linux-arm64-2.273.5.tar.gz
```

**Configure**

- Create the runner and start the configuration experience. This will ask certain question, only enter the work directory name as **express-backend** as shown below in the screenshot

```sh
./config.sh --url https://github.com/desaijay315/nodejs-github-actions --token ACHKWYI4JIGADETGKLTSH4S7RQ5YA
```

![ubuntu_10.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603023314000/OMRdsVvkU.png)


- Run this as the service!

```sh
sudo ./svc.sh install
```

![svc_12.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603023254513/fAvId1hwx.png)

- Start the service

![svc_start-12.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603023236982/LPE5x5C_j.png)


*Now, let's go to the folder where our code for this application resides and install pm2
*

**PM2**

*PM2 is the process manager for node.js applications that helps you keep the application online. This is logs facility and many salient features. Visit  [here](https://pm2.keymetrics.io/docs/usage/quick-start/)  to learn more about it
*

![pm2_i9nstall_14.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603023625155/VkpED-7QP.png)

```sh
npm i -g pm2
```

*Above command might give you some problem if the user permissions are not set properly on the node_modules and we don't want to use sudo to run the above command to install pm2 and if it does so, we can run below commands to minimize permission error issues(Alternatively you can use nexus registry to install all the global node modules)*

```sh
#create the folder on the home directory
mkdir ~/.npm-global

#configure npm to use the new directory path:
npm config set prefix '~/.npm-global'

#Open profile file with vim and insert below line:
vim ~/.profile
export PATH=~/.npm-global/bin:$PATH

#update the system variables
source ~/.profile
```

**Symlinking pm2**

```sh
sudo ln -s /home/desaijay/.npm-global/bin/pm2 /usr/bin/pm2
```


- To start our app with pm2, run the below command

```sh
pm2 start src/server.js --name=API
```

- You must see our application is up and running in the browser for the port 3000.

![localhosr_3000.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603024905476/NKDF_JBFd.png)


### Web Server  - Nginx

*Nginx is one of the most popular web servers in the world and is responsible for hosting some of the largest and highest-traffic sites on the internet. It is more resource-friendly and can be used as a web server or reverse proxy.
*

**Now, let us install Nginx and configure it.**

```sh
sudo apt install Nginx
```

- Check the status 

```sh
sudo systemctl status Nginx
```

- It should be active, and if it's not, you can start Nginx by running below command

```sh
sudo systemctl start Nginx
```

![nginx_welcome.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603037512108/yqibJSMTq.png)

- In an ideal scenario, we would like our APIs to be accessed without the port from the browser and any random port on our machine must not be allowed to access. 
- Currently, it's enabled like this - **http://13.127.99.34:3000/api ** and we would be accessing our API like this - http://13.127.99.34/api. 
- We would create a custom configuration and rewrite the URL in the location directive and leverage Nginx reverse proxy feature

- Add the below server block inside the */etc/nginx/sites-available/default* file

```sh
sudo vim /etc/nginx/sites-available/default
```

**- Nginx Reverse Proxy  + Rewrite URL**

```sh
location /api {
    rewrite ^\/api\/(.*)$ /api/$1 break;
    proxy_pass http://localhost:3000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
}
```

- We will configure some custom settings on our firewall. We will use UFW (Uncomplicated Firewall) commands and is available by default on ubuntu LTS version greater than 8.04

- Check the status of the ufw 

```sh
sudo ufw status 
```
- If it is disabled, then let's activate the ufw

```sh
sudo ufw enable 
```
- Run below commands to enable the Nginx server to take all the requests

```sh
ufw app update Nginx
ufw allow 'Nginx Full'
``` 

**Nginx Full:**  This profile opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic)

- We would be denying the port 3000 as we don't want to keep any ports open on our server and leverage Nginx reverse proxy configuration 

```sh
sudo ufw deny 3000
```

**- Status should look something like below**

![deny_3000.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603037565288/ke2lRnHGZ.png)

- Restart the Nginx server

```sh
sudo systemctl restart Nginx
```

We should now see that services are being proxied by the same server and can be accessed without the port.

![without_port.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603027708614/EVxs0tzKI.png)


**References**

 - [Workflow Syntax for GitHub Actions](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions) 
 - [Events that trigger workflows](https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows)
 - [Managing workflow runs](https://docs.github.com/en/free-pro-team@latest/actions/managing-workflow-runs)
 - [Context and Expression syntax for Github Actions](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions)



### Summary

***Overall, my opinion is that GitHub Actions is worth a try. By giving your development teams such tools and mandates to bring operational thinking to their work, organizations can help the team focus on important initiatives and gain endless amounts of benefits.***

In this article, we learned how to automate our application deployments with GitHub actions. We learned how to leverage web servers like Nginx. 

Until then feel free to connect, and leave your responses below and reach out to me if you face any issues. 

You can find me on  [Twitter](https://twitter.com/BeingJayDesai)

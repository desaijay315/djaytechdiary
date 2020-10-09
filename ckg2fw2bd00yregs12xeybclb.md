## What is docker and Why it is so popular?

Docker has grown rapidly in popularity and has been adopted by many companies and teams. In this article, we will discuss why you should use Docker and some of its core principles.

Before getting started with the Docker, let’s understand what the difference is between virtualization and containerization.

### Pre-Virtualization


![Untitled design (7).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1602163378187/Mt6zknZpw.png)

There was a time when deploying an application to a server was quite expensive. We had to purchase server racks and purchase different servers in order to deploy each of the application. We might end up using minimal resources, CPU, or memory. Because of this, the resources on each of these servers were wasted.

The side effects were directly proportional to the deployment time which was often slow, the migration was painful, and it required a significant amount of configuration and manual interventions.

As seen in the image below, we have our physical server, a host operating system, and the application deployed on top of it.

### Hypervisor Virtualization


![Untitled design (6).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1602163304489/dBRQr0LSf.png)

Hypervisor refers to the virtual machines which became quite popular among the organizations who wanted to utilize the power of installing multiple virtual machines on top of a single machine. For example, installing Virtualbox and VMware to on-premises servers into an isolated environment.

With our current technology, this can be done by using cloud-based services such as Amazon web services (AWS) or Microsoft Azure and can easily install VMs and are based on pay as you go model with no upfront cost.

The problem with hypervisor virtualization is that we need to install different operating systems across several hypervisors (virtual machines). This means we are running each instance of the kernels for those applications which is expensive.

### Container-Based Virtualization

![Untitled design (5).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1602163231172/hXCIBNFoM.png)

A container is an instance of the image which runs a program within its own set of hardware and physical resources, memory and networks in its own isolated environment.

Containers share a single host operating system, making it much more efficient for fast deployments and portability. We require only those components which are packaged inside the container within the application. We run different applications within different containers and isolate the run time environments. Containers consume less CPU and memory compared to virtual machines, and it is more cost-effective.

## **What is Docker?**

Docker is the open-source tool designed for the Developers, DevOps, and SysAdmins to build, ship, and run distributed applications on virtual machines any computer — whether it’s your laptop or in the cloud.

## Getting started with Docker

To install docker on your machine (macOS/Linux/Windows), please visit the official website and follow the instructions.


%[https://www.docker.com]

### Docker Images

Docker images are read-only templates with instructions for creating a Docker container. It defines container code, libraries, environment variables, configuration files and more.

### Dockerfile

It will outline how the image will create a container

### Docker Hub

Docker Hub is the repository of private and public docker images. If you have Docker images that need to be installed on the client-server and run the containers, please go with Docker Hub.

### Let’s Explore the Docker Commands


- To verify the version

This command helps to get the current version of docker running on your machine


```
docker —-version
```

<noscript><img alt="Image for post" class="t u v hc aj" src="https://miro.medium.com/max/1588/1*nkmTUnHhuHciV6_yI6t3zw.png" width="794" height="196" srcSet="https://miro.medium.com/max/552/1*nkmTUnHhuHciV6_yI6t3zw.png 276w, https://miro.medium.com/max/1104/1*nkmTUnHhuHciV6_yI6t3zw.png 552w, https://miro.medium.com/max/1280/1*nkmTUnHhuHciV6_yI6t3zw.png 640w, https://miro.medium.com/max/1400/1*nkmTUnHhuHciV6_yI6t3zw.png 700w" sizes="700px"/></noscript>


- Search for a docker image


```
docker search <image>
``` 


- Download the images without running the container, use

```
docker pull <image name>
``` 

This command pulls the docker image from Docker Hub. Below, we pull the Redis image which can be further used if we want to run a Redis container in future

**_latest-_** _This tag means we are pulling the latest image from the docker hub_

<noscript><img alt="Image for post" class="t u v hc aj" src="https://miro.medium.com/max/3216/1*hHBC81hsHgI1sGbY1xH91Q.png" width="1608" height="648" srcSet="https://miro.medium.com/max/552/1*hHBC81hsHgI1sGbY1xH91Q.png 276w, https://miro.medium.com/max/1104/1*hHBC81hsHgI1sGbY1xH91Q.png 552w, https://miro.medium.com/max/1280/1*hHBC81hsHgI1sGbY1xH91Q.png 640w, https://miro.medium.com/max/1400/1*hHBC81hsHgI1sGbY1xH91Q.png 700w" sizes="700px"/></noscript>

- docker run container

As we saw, pulling the images has already downloaded the image and installed them on our machine, and Docker will not download the image again and run only the Redis container.

```
docker run redis
```

<noscript><img alt="Image for post" class="t u v hc aj" src="https://miro.medium.com/max/4888/1*sBjMpyoHNom23tHI3CEwTw.png" width="2444" height="572" srcSet="https://miro.medium.com/max/552/1*sBjMpyoHNom23tHI3CEwTw.png 276w, https://miro.medium.com/max/1104/1*sBjMpyoHNom23tHI3CEwTw.png 552w, https://miro.medium.com/max/1280/1*sBjMpyoHNom23tHI3CEwTw.png 640w, https://miro.medium.com/max/1400/1*sBjMpyoHNom23tHI3CEwTw.png 700w" sizes="700px"/></noscript>


Now, let’s verify whether the Redis is actually working in the Docker environment.

We will execute the command in the running the container, so without closing the above tab, open the new one and execute the following command

```
docker ps
```

- Execute an interactive bash shell on the container

```
docker exec -it <container id> <command>
```

<noscript><img alt="Image for post" class="t u v hc aj" src="https://miro.medium.com/max/4820/1*3vxf5B9uUGCl6sHV2j5ZQg.png" width="2410" height="536" srcSet="https://miro.medium.com/max/552/1*3vxf5B9uUGCl6sHV2j5ZQg.png 276w, https://miro.medium.com/max/1104/1*3vxf5B9uUGCl6sHV2j5ZQg.png 552w, https://miro.medium.com/max/1280/1*3vxf5B9uUGCl6sHV2j5ZQg.png 640w, https://miro.medium.com/max/1400/1*3vxf5B9uUGCl6sHV2j5ZQg.png 700w" sizes="700px"/></noscript>

**-it flag —** As docker runs inside the Linux environment, every process we create has three channels attached to it STDIN, STDOUT, STDERR. So every command you write to the terminal, it initiates the STDIN channel, which provides inputs to the process. STDOUT conveys the output which you would see in the terminal. STDERR conveys the error which occurs while running the processes.

- Show all the running and stopped containers

```
docker ps
``` 

- To see all the running containers

```
docker ps -a

``` 

- List all the images

```
docker image ls
```

<noscript><img alt="Image for post" class="t u v hc aj" src="https://miro.medium.com/max/3640/1*Yeo5xhVnE3WsXLB4EzpUwg.png" width="1820" height="404" srcSet="https://miro.medium.com/max/552/1*Yeo5xhVnE3WsXLB4EzpUwg.png 276w, https://miro.medium.com/max/1104/1*Yeo5xhVnE3WsXLB4EzpUwg.png 552w, https://miro.medium.com/max/1280/1*Yeo5xhVnE3WsXLB4EzpUwg.png 640w, https://miro.medium.com/max/1400/1*Yeo5xhVnE3WsXLB4EzpUwg.png 700w" sizes="700px"/></noscript>

- List all running containers

```
docker container ls -a

``` 

- Stop the container

```
docker stop <container id>
```

<noscript><img alt="Image for post" class="t u v hc aj" src="https://miro.medium.com/max/4892/1*aa2Dm0cfgYDaK7XBeg9oeQ.png" width="2446" height="372" srcSet="https://miro.medium.com/max/552/1*aa2Dm0cfgYDaK7XBeg9oeQ.png 276w, https://miro.medium.com/max/1104/1*aa2Dm0cfgYDaK7XBeg9oeQ.png 552w, https://miro.medium.com/max/1280/1*aa2Dm0cfgYDaK7XBeg9oeQ.png 640w, https://miro.medium.com/max/1400/1*aa2Dm0cfgYDaK7XBeg9oeQ.png 700w" sizes="700px"/></noscript>

- Command to removes images. If the container is still running, you will get the following error.

```
docker rmi images & docker rm container
``` 

<noscript><img alt="Image for post" class="t u v hc aj" src="https://miro.medium.com/max/4896/1*RTaAKdswbnjut2ah34qivA.png" width="2448" height="138" srcSet="https://miro.medium.com/max/552/1*RTaAKdswbnjut2ah34qivA.png 276w, https://miro.medium.com/max/1104/1*RTaAKdswbnjut2ah34qivA.png 552w, https://miro.medium.com/max/1280/1*RTaAKdswbnjut2ah34qivA.png 640w, https://miro.medium.com/max/1400/1*RTaAKdswbnjut2ah34qivA.png 700w" sizes="700px"/></noscript>

*The condition here is to first remove the running container which uses this particular image and then remove the image.
*

<noscript><img alt="Image for post" class="t u v hc aj" src="https://miro.medium.com/max/4884/1*QJsFTLM1YozNSErfpzn2Kg.png" width="2442" height="744" srcSet="https://miro.medium.com/max/552/1*QJsFTLM1YozNSErfpzn2Kg.png 276w, https://miro.medium.com/max/1104/1*QJsFTLM1YozNSErfpzn2Kg.png 552w, https://miro.medium.com/max/1280/1*QJsFTLM1YozNSErfpzn2Kg.png 640w, https://miro.medium.com/max/1400/1*QJsFTLM1YozNSErfpzn2Kg.png 700w" sizes="700px"/></noscript>

- Remove it by running the command

```
docker rm <container id>
```

We have successfully removed all the containers running the Redis image and then removed the Redis image by running the command:

```
docker rmi redis
```

- Remove all the stopped container

We have seen how to stop the container, but what if we want to remove all the stopped containers?

- Run the command to stop all the running containers

```
docker system prune
```

<noscript><img alt="Image for post" class="t u v hc aj" src="https://miro.medium.com/max/2644/1*3C5zw45WCWqoLVPlaOtSrA.png" width="1322" height="582" srcSet="https://miro.medium.com/max/552/1*3C5zw45WCWqoLVPlaOtSrA.png 276w, https://miro.medium.com/max/1104/1*3C5zw45WCWqoLVPlaOtSrA.png 552w, https://miro.medium.com/max/1280/1*3C5zw45WCWqoLVPlaOtSrA.png 640w, https://miro.medium.com/max/1400/1*3C5zw45WCWqoLVPlaOtSrA.png 700w" sizes="700px"/></noscript>

This will not only remove the stopped containers but also removes the build caches. This you will need to again pull the images from the Docker Hub next time you run them.

- Remove all Images

```
docker image ls -aq | xargs docker rmi -f
```

- Remove all containers


``` 
docker container ls -aq | xargs docker container rm
``` 


### Conclusion

In this article, we have learned application deployment on virtual machines (pre and post virtualization), containerization, and some docker commands which are useful in day to day activities.

If you liked it please leave some claps to show your support. Also, leave your responses below and reach out to me if you face any issues.

> _Follow me on_ [_Twitter_](http://www.twitter.com/beingjaydesai) _| Check out my_ [_LinkedIn_](https://www.linkedin.com/in/iamjaydesai/) _| See my_ [_GitHub_](https://github.com/desaijay315)


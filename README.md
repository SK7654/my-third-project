![0](https://user-images.githubusercontent.com/64473684/85363166-d4708d80-b53d-11ea-9b01-6ce997df3f02.jpg)

#Rolling_Updates_with_Dynamic_Distributed_Clusters

In this article I am going to show " How to roll out updates to your web page with zero downtime using Dynamic Distributed Clusters". I recently completed this project with help of Jenkins, Docker and Kubernetes.

PREREQUISITE:

1.RHEL 8 (GUI/CLI)

2.Docker-CE

3.Git

4.Jenkins

5.Kubernetes (Should be configured on Windows and RHEL)

We will start by creating a repository on GitHub. We will use web-hook in GitHub so that whenever developer pushes any update, Job 1 will automatically starts building on Jenkins

![1](https://user-images.githubusercontent.com/64473684/85364323-84df9100-b540-11ea-8c26-52200f192d28.PNG)

Now we have to build a docker image which will have Kubernetes installed and running on top of Cent OS. We also need to install Git, Java, SSH-Server and some certificates which are required in config file of Kubectl.


![0](https://user-images.githubusercontent.com/64473684/85364844-a8570b80-b541-11ea-89d0-b75d4d91681e.png)

![0 (1)](https://user-images.githubusercontent.com/64473684/85364852-abea9280-b541-11ea-925e-b796b37a3db8.png)

We need to allow Docker daemon to be accessed remotely. Since docker is isolated. For that we need to edit /usr/lib/systemd/system/docker.service.

Here 0.0.0.0 means that you can denote it with any IP given to the DOCKER HOST and you can assign this IP with any available port.


![0 (2)](https://user-images.githubusercontent.com/64473684/85364863-b3aa3700-b541-11ea-982e-033dc6b8b9d1.png)

After this process, Restart the services
1.	systemctl daemon-reload
2.	systemctl restart docker.service
Now we need to export DOCKER_HOST. Use command
1.	export DOCKER_HOST=<your IP address>:<port you assigned>
2.	echo $DOCKER_HOST
Now we need to setup Docker Cloud on Jenkins. Before that we need to download some plugins on Jenkins
1.	DOCKER
2.	GITHUB
3.	WORKSPACE CLEANUP
Now to setup Docker cloud on Jenkins. Go to following section.
Manage Jenkins > Manage Nodes and Clouds > Configure Clouds > Add a new cloud
  
  ![2](https://user-images.githubusercontent.com/64473684/85367016-1271af80-b546-11ea-9f1f-d89ecc62b3cf.PNG)
  
Docker Host URI is the remote docker host where all the containers run.

Now comes the JOB1:

In this job it will download the files from GitHub using repository URL. The GitHub repository contains Dockerfile 2 and HTML file. The workspace will be cleaned before every build and then it will pull repository. Job 2 will build after the successful build Job 1.

In the Build action we are using Build / Publish Docker Image. In this directory is set to default workspace. You need to give name of docker cloud which you assigned in the managed nodes and clouds. Give your docker image name and give credentials of Docker Hub and then enable push image.

Dockerfile_2

![0](https://user-images.githubusercontent.com/64473684/85367280-99268c80-b546-11ea-92bf-d6048bcb8c5b.png)

![Captur3](https://user-images.githubusercontent.com/64473684/85367867-b871e980-b547-11ea-8520-89d60ad62941.PNG)

OUTPUT OF JOB 1:

![4](https://user-images.githubusercontent.com/64473684/85368046-1ef70780-b548-11ea-841a-e861d05d8e4d.PNG)

As you can see my image is successfully pushed to Docker Hub.

![0 (6)](https://user-images.githubusercontent.com/64473684/85368268-9167e780-b548-11ea-81be-366c924e1590.png)

The files used in Job 1 are present in this GitHub repository.

Config file used in Dockerfile 1 for kubectl


![0 (7)](https://user-images.githubusercontent.com/64473684/85369246-5e265800-b54a-11ea-8409-171711e3b9d8.png)

Now we will add Docker template in the Docker Cloud settings. In Docker template we need to give a label which we will use during configuration of Job 2 and we need to give right path of our Dockerfile 1. In container settings we need to give right path of the config file which is present in your RHEL.

Container Settings > Volumes

/root/task3:/root/.kube/

**Right side is the path where we have mounted config file that is to be used in Docker container. Left side is the path where we have mounted config file in RHEL.

![5](https://user-images.githubusercontent.com/64473684/85369552-e99fe900-b54a-11ea-8881-750901bfeade.PNG)

JOB 2:

This job will be build after successful build of Job 1. This job has to run on dynamic container slave for that we have already added a template on Docker cloud. This job will create a new deployment (if it does not exist) or else we will roll out update to the existing deployment. Since we our building this job for first time, So it will create a new deployment.

![0 (9)](https://user-images.githubusercontent.com/64473684/85369821-6b901200-b54b-11ea-824b-b06797b4d8c0.png)

It will use label which we have created on Docker cloud.

![0 (10)](https://user-images.githubusercontent.com/64473684/85370469-64b5cf00-b54c-11ea-90b5-d30296c5d6a8.png)

To check whether kubectl is configured properly or not, We will use command "kubectl --help" and then we will write the code which will help in creating new deployment.

**Before running this job, Make sure to start minikube from windows command line and kubectl should be properly configured on RHEL and make sure you have good internet speed.

Output of JOB 2:




















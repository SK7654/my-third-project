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












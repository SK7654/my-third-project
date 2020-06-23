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





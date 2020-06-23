![0](https://user-images.githubusercontent.com/64473684/85363166-d4708d80-b53d-11ea-9b01-6ce997df3f02.jpg)
Rolling Updates with Dynamic Distributed Clusters
In this article I am going to show " How to roll out updates to your web page with zero downtime using Dynamic Distributed Clusters". I recently completed this project with help of Jenkins, Docker and Kubernetes.

PREREQUISITE:

RHEL 8 (GUI/CLI)
Docker-CE
Git
Jenkins
Kubernetes (Should be configured on Windows and RHEL)
We will start by creating a repository on GitHub. We will use web-hook in GitHub so that whenever developer pushes any update, Job 1 will automatically starts building on Jenkins


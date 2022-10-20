# jenkins
Jenkins is an open source automation server. It helps automate the parts of software development related to building, testing, and deploying, facilitating continuous integration and continuous delivery. It is a server-based system that runs in servlet containers such as Apache Tomcat. 

# Setup
## Installation
1. [Click here](https://github.com/vinaykagithapu/dockerDepolyments/blob/main/jenkins/README.md) to install jenkins on docker.
2. [Click here](https://github.com/vinaykagithapu/kubernetesDeployments/tree/main/helm/jenkins) to deploy the jenkins helm chart in kubernetes cluster. 

## Post Installation
1. [Click here](jenkinsWithAAD/README.md) to configure jenkins with Azure Active Directory for authentication and authorization.
2. [Click here](jenkinsWithK8s/README.md) to configure jenkins to run builds in kubernetes pods. 

# Projects
1. [Click here](sampleProject/README.md) to create a sample project.
2. [Click here](sampleProjectBuildOnK8s/README.md) to create a project to run java app with in the kubernetes pods. 
3. [Click here](pipelineProject/README.md) to create a sample pipeline.
4. [Click here](./dockerCredsVault/README.md) to create a pipeline that build docker image and push to dockerhub using credentials stored in vault.
# Setup Jenkins Build Agents on Kubernetes Pods
Setting up jenkins build pods on kubernetes. Pod will be created when a build is triggered and destroy after the build. 

## Requirements
1. A working Kubernetes cluster. 
2. A running Jenkins master 
3. Jenkins **Kubernetes** Plugin is required to set up Kubernetes-based build agents.

## 1. Install Jenkins Kubernetes Plugin
1. Click on **Dashboard > Manage Jenkins > Manage Plugins**
2. Click on **Available** > search for **Kubernetes** Plugin and install it. 

## 2. Create a Kubernetes Cloud Configuration
1. Go to **Dashboard > Manage Jenkins > Manage Nodes and Clouds**
2. Click on **Configure Clouds > Add a new Cloud > Kubernetes > Kubernetes Cloud Details**
3. Here we have two scenarios:
   1. Jenkins server running inside the same Kubernetes cluster 
   2. It’s server running out the Kubernetes cluster.
4. We are considering **scenario one** here.
5. Click on **Test connection**. It should show a **connected** message if the pod can connect to the Kubernetes master API. 

## 3. Configure the Jenkins URL Details
1. Agent pods can connect to the cluster via internal service DNS
2. So, we need to configure **Jenkins URL** as
```
http://jenkins.jenkins.svc.cluster.local:8080
```
```shell
http://<service-name>.<namespace>.svc.cluster.local:8080
```
3. **Jenkins tunnel** as
```
jenkins-agent:50000
```
```shell
<service-name>-agent:50000
```
4. Add the **POD label**
```
Key   : jenkins 
Vaule : slave
```

## 5. Create POD and Container Template
1. Click on **Pod Templates > Add Pod Template > Pod Template details**
```
Name      : kube-agent
Namespace : jenkins
Labels    : kubeagent
```
2. Click on **Apply > Save**

## 5. Testing - Create a Freestyle Project.
1. Goto **Dashboard > New Item > Name: test-project > Freestyle project > OK**
2. In general section, tick the checkbox as follows:
3. [x] Restrict where this project can be run

   Label Expression : **kubeagent**

4. In build section, **Add build step > Execute shell**
```shell
echo "Testing project"
```
5. Click on **Apply > Save > Build Now**
6. In **Build Histroy** > Click on the build number
7. Goto **Console Output**, to the output of build result

# Cleanup
1. Delete project: Click on **test-project > Delete Project > OK**
2. Delect Cloud: Goto **Dashboard > Manage Jenkins > Manage Nodes and Clouds > Configure Clouds > Delete cloud > Apply > Save** 

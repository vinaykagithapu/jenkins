# Sample Project On K8s
This is a sample Freestyle project that runs the build in kubernetes pods.

## Requirements
1. Jenkins [setup](../README.md#installation)
2. Configure jenkins with kubernetes cloud [setup](../jenkinsWithK8s/README.md)

# Setup
## Create Freestyle Project
1. Goto **Dashboard > New Item > Enter an item name: projectBuildOnK8s** 
2. **Freestyle project > OK**

## Configure Freestyle Project
1. Add the Description of the project : **This is a sample free style project to run java app with in the kubernetes pods.**
2. In general section, tick the checkbox as follows:
   2. [x] Restrict where this project can be run

         Label Expression : **kubeagent**

3. Source Code Management > **Git** > Repository URL: **https://github.com/vinaykagithapu/sampleJavaApp.git**
4. Branch Specifier (blank for 'any'): ***/main**
5. **Build Steps > Add build step > Execute shell >**
```shell
javac sampleApp.java
java sampleApp
```
## Build
1. **Dashboard > projectBuildOnK8s > Build Now**
2. **Build History > #1 or #2 ... > Console Output**
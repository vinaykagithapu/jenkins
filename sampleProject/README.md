# sample Project
This is a sample Freestyle project that clone the sampleJavaApp from the github and executes the sampleApp.

## Requirements
1. Jenkins [setup](../README.md) 

# Setup
## Create Freestyle Project
1. **Dashboard > New Item > Enter an item name : sampleProject** 
2. **Freestyle project > OK**

## Configure Freestyle Project
1. Add the Description of the project : **This is a sample free style project to run java app.**
2. Source Code Management > **Git** > Repository URL: **https://github.com/vinaykagithapu/sampleJavaApp.git**
3. Branch Specifier (blank for 'any'): ***/main**
4. **Build Steps > Add build step > Execute shell >**
```shell
javac sampleApp.java
java sampleApp
```
5. **Apply > Save**

## Build 
1. **Dashboard > sampleProject > Build Now**
2. **Build History > #1 or #2 ... > Console Output**
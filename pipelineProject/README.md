# Sample Pipeline Project
This is a sample pipeline project that builds and runs the sample java app in kubernetes pods.

## Requirements
1. Jenkins [setup](../README.md#installation)
2. Configure jenkins with kubernetes cloud [setup](../jenkinsWithK8s/README.md)

# Setup
## Create Pipeline Project
1. **Dashboard > New Item > Enter an item name : samplePipeline**
2. **Pipeline > OK**

## Configure Freestyle Project
1. Add the Description of the project : **This is a sample pipeline project that builds and runs the sample java app in kubernetes pods.**
2. Pipeline > Pipeline script :
```shell
pipeline {
    agent { label 'kubeagent' }

    stages {
        stage('Build') {
            steps {
                sh 'git clone https://github.com/vinaykagithapu/sampleJavaApp.git'
                sh 'javac sampleJavaApp/sampleApp.java'
                sh 'cp sampleJavaApp/sampleApp.class .'
            }
        }
        stage('Run') {
            steps {
                sh 'java sampleApp'
            }
        }
    }
}
```
3. **Apply > Save**

## Build
1. **Dashboard > samplePipeline > Build Now**
2. **Build History > #1 or #2 ... > Console Output**
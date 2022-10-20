# Docker Credentials using Vault
This project clones the sampleDockerBuild repo from the github, builds a docker image and push the docker image to docker hub using credentials stored in vault.

## Requirements
1. Jenkins [setup](../README.md) 
2. Vault [setup](https://github.com/vinaykagithapu/kubernetesDeployments/tree/main/vault/helm)
2. Vault plugin: **Dashboard > Manage Jenkins > Manage Plugins > Available > search: vault > [*] HashiCorp Vault > Install without restart > Go back to the top page**.

# Setup
## Create Pipeline Project
1. **Dashboard > New Item > Enter an item name : TestDockerCreds**
2. **Pipeline > OK**

## Configure Pipeline Project
1. Create a repo in dockerhub (username/cafe-app).
2. Create docker creds in vault
    1. Login to vault-ui
    2. Click on Enable new engine > [*] kv > Next > Enable Engine > Create secret > Path for this secret: dockercreds > Secret data: key1-value1, Add key2-value2 > Save 
3. Add the Description of the project : **This is a sample pipeline project that builds a docker image and push image to dockerhub.**
4. Pipeline > Pipeline script :
```shell
String label = "docker-${UUID.randomUUID().toString()}" as String
podTemplate(label: label, yaml: """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker:18
    command: ['cat']
    tty: true
    volumeMounts:
    - name: dockersock
      mountPath: /var/run/docker.sock
  volumes:
  - name: dockersock
    hostPath:
      path: /var/run/docker.sock
""") {
    node(label) {
    stage ('GIT CheckOut') {
        git branch: 'main', url: 'https://github.com/vinaykagithapu/sampleDockerBuild.git'
        sh 'ls -al'
    }
    
    stage("Docker Build"){
        container('docker') {
            withVault(configuration: [timeout: 60, vaultCredentialId: 'vault-token', vaultUrl: 'http://vault-ui.vault.svc.cluster.local:8200'], vaultSecrets: [[path: 'kv/dockercreds', secretValues: [[envVar: 'username', vaultKey: 'user'], [envVar: 'password', vaultKey: 'pass']]]]) {
            sh '''
                docker build -t cyberwarrior423/cafe-app .
                docker login -u $username -p $password
                docker push cyberwarrior423/cafe-app
            '''
            }
        }
    }
}
}
```
5. **Apply > Save**

## Build
1. **Dashboard > TestDockerCreds > Build Now**
2. **Build History > #1 or #2 ... > Console Output**
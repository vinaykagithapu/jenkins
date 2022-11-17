# Kubeconfig from Vault
This project gets the kubeconfig file from the vault and uses the kubeconfig for kubectl.

## Requirements
1. Vault [setup](https://github.com/vinaykagithapu/kubernetesDeployments/tree/main/vault/helm)
2. Create kubeconfig in Vault.
3. Jenkins [setup](../README.md) 
4. Configure jenkins with k8s.
5. Vault plugin: **Dashboard > Manage Jenkins > Manage Plugins > Available > search: vault > [*] HashiCorp Vault > Install without restart > Go back to the top page**.
6. Vault creds: **Dashboard > Manage Jenkins > Manage Credentials > System > Global credentials (unrestricted) > Add Credentials > kind: Vault Token Credentials > Token: <token> > ID: vault-token > Description: vault-token**.


# Setup
## Create Pipeline Project
1. **Dashboard > New Item > Enter an item name : TestKubeConfig**
2. **Pipeline > OK**

## Configure Pipeline Project
1. Add the Description of the project : **This is a sample pipeline project that gets the kubeconfig file from the vault and uses the kubeconfig for kubectl.**
4. Pipeline > Pipeline script :
```shell
String label = "kubeconfig-pod" as String
podTemplate(label: label, yaml: """
apiVersion: v1
kind: Pod
metadata:
  annotations:
    vault.hashicorp.com/agent-inject: "true"
    vault.hashicorp.com/tls-skip-verify: "true"
    vault.hashicorp.com/agent-inject-secret-helloworld: "secret/secrets-as-env/helloworld"
    vault.hashicorp.com/agent-inject-template-helloworld: |
      {{ with secret "secret/secrets-as-env/helloworld" -}}
      {{ .Data.kubeconfig }}
      {{- end }}
    vault.hashicorp.com/role: "secrets-as-env-role"
  labels:
    app: secrets-as-env
spec:
  serviceAccountName: secrets-as-env
  containers:
  - name: kubectl
    image: bitnami/kubectl:1.20
    command: ['cat']
    env:
    - name: KUBECONFIG
      value: "/vault/secrets/helloworld"
    tty: true
    securityContext:
      runAsUser: 1000
""") {
    node(label) {
      stage('Get KubeConfig') {
        container('kubectl') { 
          sh'''
            echo $KUBECONFIG
            # cat /vault/secrets/helloworld
            # kubectl version
            kubectl -n cert-manager get pods
          '''
        }
      }
    }
  }

```
5. **Apply > Save**

## Build
1. **Dashboard > TestDockerCreds > Build Now**
2. **Build History > #1 or #2 ... > Console Output**
pipeline {
    agent any

    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/raulgomezn/netcore50-api-test'
                
            }
        }
        stage('Build') {
            steps {
                bat(script: 'dir', returnStdout: true);
                bat(script: 'dotnet restore', returnStdout: true);
                bat(script: 'dotnet build', returnStdout: true);
                bat(script: 'dotnet test', returnStdout: true);
            }
        }
        stage('Docker') {
            steps {
                bat(script: 'docker login --username %UsernameDockerHub% --password %PasswordDocker%', returnStdout: true);
                bat(script: 'docker build -t rgomezn/servicenet5 .', returnStdout: true);
                bat(script: 'docker push rgomezn/servicenet5', returnStdout: true);
            }
        }
        stage('Deploy Dev') {
            steps {
                bat(script: 'az login --service-principal --username 85beb53c-206f-434a-95f9-701f53801b2d --password Ypv_0Dw-5xpvfqjise5lhWiJMaHlX.O0yZ --tenant cb731c0f-7a39-4a91-92e8-7babf0b6d890', returnStdout: true);
                bat(script: 'az account set --subscription "Azure for Students"', returnStdout: true);
                bat(script: 'az container restart --name micro5testservice-rgn --resource-group aforo-terraform', returnStdout: true);
                //bat(script: 'az container restart --name micro5testservice-rgn --resource-group aforo-terraform', returnStdout: true);
                //bat(script: 'az container create --resource-group aforo255Devops --name micro5testservice --image antony0618/servicenet5:latest --dns-name-label micro5testservice --ports 80', returnStdout: true);
            }
        }
        stage('Deploy Prod') {
            steps {
                //bat(script: 'az aks get-credentials --resource-group aforo255Devops --name aforo255jenkins & kubectl config get-contexts --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
                bat(script: 'az aks get-credentials --resource-group  aforo-terraform --name cluster_devops & kubectl config get-contexts --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
                bat(script: 'kubectl config use-context cluster_devops --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
                bat(script: 'Kubectl delete --all pods --kubeconfig=%KUBE_PATH_CONFIG% & kubectl apply -f k8s.yml --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
            }
        }
    }
}

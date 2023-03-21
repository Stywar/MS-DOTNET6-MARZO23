pipeline {
    agent any

    stages {
        stage('SCM') {
            steps {
               git branch: 'master', url: 'https://github.com/Stywar/MS-DOTNET6-MARZO23'
            }
        }
        
          stage('Build Net6.0') {
            steps {
                bat(script: 'dir' , returnStdout:true);
                bat(script: 'dotnet restore' , returnStdout:true);
                bat(script: 'dotnet build' , returnStdout:true);
                bat(script: 'dotnet test' , returnStdout:true);
            }
        }
        
          stage('Docker Build') {
            steps {
            //bat(script: 'docker build -t antony0618/dotnet6v3 .' , returnStdout:true);   
            bat(script: 'docker build -t antony0618/dotnet6v3 .' , returnStdout:true);
            bat(script: 'docker push antony0618/dotnet6v3' , returnStdout:true);  
            }
        }
        
            stage('Deploy AKS ') {
            steps {
            bat(script: 'az aks get-credentials --resource-group Devops --name aksdevops23  & kubectl config get-contexts --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
            bat(script: 'kubectl config use-context aksdevops23  --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
            bat(script: 'kubectl apply -f k8s.yml --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
            bat(script: 'kubectl rollout restart deployment app-deployment --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
            }
        }
    }
}

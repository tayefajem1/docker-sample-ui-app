pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = "https://hub.docker.com/repository/docker/tayefajem"
        DOCKER_REPO = "hanniel"
        BUILD_NUMBER = "1.0.0"
        DOCKER_IMAGE = "tayefajem/${DOCKER_REPO}"
        DOCKER_USERNAME = "tayefajem"
     
        
    }

    stages {
        stage('Clone repository') {
            steps {
                git branch: 'main', url: 'https://github.com/HandsOnDevOpsTraining/docker-sample-ui-app.git'
            }
        }
         //stage('Build Code') {
            //steps {
                // Build the Java project using Maven
               // sh 'mvn clean package'
            //}
        //}

        //stage('Test Code') {
            //steps {
                // Run unit tests using Maven
                //sh 'mvn test'
            //}
       // }
        stage('Build Docker image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'docker-credentials-id', variable: 'DOCKER_PASSWORD')]) {
                    sh """
                        echo "${DOCKER_PASSWORD}" | docker login -u ${DOCKER_USERNAME} --password-stdin
                        docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}
                    """
                }
            }
        }
      stage ('K8S Deploy') {
        steps {
           script {
                //certificate-authority-data: caCertificate (copy all except certificate-authority-data)
               //Use commands for Kubernetes endpoint which is serverUrl: kubectl cluster-info
               //config_ola: ~/.kube/config. This can be got after the set up of your kubernetes and kubectl in the vm where kubectl is installed which your jenkin server (use command ~/.kube/config) 
                kubeconfig(caCertificate: 'kubernetes_certificate', 
                credentialsId: 'config_ola', 
                serverUrl: 'https://kubernetes-dns-whkp8id6.hcp.eastus.azmk8s.io:443') {
                sh 'kubectl apply -f kubernetes-my-sample-ui-deployment-LoadBalancer.yaml'

	          	}
                    
            }
        }
     }

    }
}


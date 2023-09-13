pipeline {
    agent any 

    stages {
        stage("code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/dheeraj07-borawat/nodejs-k8sinfra.git", branch: "main"
            }
        }
        
        stage("build") {
            steps {
                echo "Building the Docker image"
                sh "docker build -t my-nodejs-app:latest ."
            }
        }
        
        stage("push to dh") {
            steps {
                echo "Pushing the Docker image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: 'vanshu07', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "docker tag my-nodejs-app \${DOCKER_USERNAME}/my-nodejs-app:latest"
                    sh "docker login -u \${DOCKER_USERNAME} -p \${DOCKER_PASSWORD}"
                    sh "docker push \${DOCKER_USERNAME}/my-nodejs-app:latest"
                }
            }
        }

        // stage("deploy to k8s") {
        //     steps {
        //         echo "Deploying to Minikube cluster"
        //         sh "kubectl apply -f app.yml"
        //         sh "kubectl apply -f service.yml"
        //     }
        // }
        
        stage("deploy to k8s") {
            steps {
                echo "Deploying to Minikube cluster"
                withCredentials([file(credentialsId: 'my_kubernetes', variable: 'KUBECONFIG')]) {
                    // sh "export KUBECONFIG=\${KUBECONFIG} && kubectl apply -f app.yml && kubectl apply -f service.yml"
                    sh "export KUBECONFIG=/usr/bin/jenkins && kubectl apply -f app.yml && kubectl apply -f service.yml"
                }
            }
        }
    }
}

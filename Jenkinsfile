// pipeline {
//     agent any 

//     stages {
//         stage("code") {
//             steps {
//                 echo "Cloning the code"
//                 git url: "https://github.com/dheeraj07-borawat/nodejs-k8sinfra.git", branch: "main"
//             }
//         }
        
//         stage("build") {
//             steps {
//                 echo "Building the Docker image"
//                 sh "docker build -t my-nodejs-app:latest ."
//             }
//         }
        
//         stage("push to dh") {
//             steps {
//                 echo "Pushing the Docker image to Docker Hub"
//                 withCredentials([usernamePassword(credentialsId: 'vanshu07', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
//                     sh "docker tag my-nodejs-app \${DOCKER_USERNAME}/my-nodejs-app:latest"
//                     sh "docker login -u \${DOCKER_USERNAME} -p \${DOCKER_PASSWORD}"
//                     sh "docker push \${DOCKER_USERNAME}/my-nodejs-app:latest"
//                 }
//             }
//         }

//         // stage("deploy to k8s") {
//         //     steps {
//         //         echo "Deploying to Minikube cluster"
//         //         sh "kubectl apply -f app.yml"
//         //         sh "kubectl apply -f service.yml"
//         //     }
//         // }
        
//         stage("deploy to k8s") {
//             steps {
//                 echo "Deploying to Minikube cluster"
//                 withCredentials([file(credentialsId: 'my_kubernetes', variable: 'KUBECONFIG')]) {
//                     // sh "export KUBECONFIG=\${KUBECONFIG} && kubectl apply -f app.yml && kubectl apply -f service.yml"
//                     sh "export KUBECONFIG=/usr/bin/jenkins"
//                     sh "export KUBECONFIG=${KUBECONFIG}"
//                     sh "kubectl apply -f app.yml && kubectl apply -f service.yml"
//                 }
//             }
//         }
//     }
// }




pipeline {

  environment {
    dockerimagename = "vanshu07/my-nodejs-app:latest"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/dheeraj07-borawat/nodejs-k8sinfra.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

  }

}
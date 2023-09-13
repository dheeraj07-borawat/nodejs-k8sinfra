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
                    sh "docker tag my-nodejs-app ${DOCKER_USERNAME}/my-nodejs-app:latest"
                    sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                    sh "docker push ${DOCKER_USERNAME}/my-nodejs-app:latest"
                }
            }
        }
        
        stage("deploy to k8s") {
            steps {
                echo "Deploying to Kubernetes cluster"
                withCredentials([file(credentialsId: 'my_kubernetes', variable: 'KUBECONFIG')]) {
                    sh """
                    export KUBECONFIG=\${KUBECONFIG}
                    kubectl apply -f app.yml --kubeconfig=\${KUBECONFIG}
                    """
                }
            }
        }
    }
}






















































































// pipeline {
//     agent any
    
    // stages {
    //     stage('Checkout') {
    //         steps {
    //             // Checkout the source code from your version control system (e.g., Git)
    //             checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/dheeraj07-borawat/nodejs-k8sinfra.git']]])
    //             checkout scm
    //         }
    //     }

    //     stage('Build') {
    //         steps {
    //             // Build the Node.js application
    //             sh 'npm install' // Or use yarn if you prefer
    //         }
    //     }

    //     stage('Test') {
    //         steps {
    //             // Run tests if you have them
    //             sh 'npm test'
    //         }
    //     }

    //     stage('Build Docker Image') {
    //         steps {
    //             // Build a Docker image for the Node.js application
    //             sh 'docker build -t my-nodejs-app .'
    //         }
    //     }

    //     stage('Push Docker Image') {
    //         steps {
    //             // Push the Docker image to a container registry (e.g., Docker Hub)
    //             withCredentials([usernamePassword(credentialsId: 'your-docker-hub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
    //                 sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
    //                 sh 'docker push my-nodejs-app'
    //             }
    //         }
    //     }

    //     stage('Deploy') {
    //         steps {
    //             // Deploy the Docker image to your Kubernetes cluster
    //             sh 'kubectl apply -f app.yaml'
    //             // sh 'kubectl apply -f service.yaml'
    //         }
    //     }
    // }

//     post {
//         success {
//             echo 'Pipeline succeeded!'
//         }
//         failure {
//             echo 'Pipeline failed!'
//         }
//     }
// }


// pipeline {
//     agent any
    
//     stages {
//         stage('Git Clone') {
//             steps {
//                 git credentialsId: 'GIT_CREDENTIALS', url: 'https://github.com/dheeraj07-borawat/nodejs-k8sinfra.git'
//             }
//         }

//         stage('Build') {
//             steps {
//                 // Build the Node.js application
//                 sh 'npm install' // Or use yarn if you prefer
//             }
//         }
        
//         stage('Build Docker Image') {
//             steps {
//                 script {
//                   sh 'docker build -t vanshu07/my-nodejs-app .'
//                 }
//             }
//         }
        
//         stage('Deploy Docker Image') {
//             steps {
//                 script {
//                  withCredentials([string(credentialsId: 'vanshu07', variable: 'dockerhubpwd')]) {
//                     sh 'docker login -u vanshu07 -p ${dockerhubpwd}'
//                  }  
//                  sh 'docker push vanshu07/my-nodejs-app'
//                 }
//             }
//         }
    
//         stage('Deploy App on k8s') {
//             steps {
//                 withCredentials([
//                     string(credentialsId: 'my_kubernetes', variable: 'api_token')
//                 ]) {
//                     sh 'kubectl --token $api_token --server https://192.168.103.2:8443 --insecure-skip-tls-verify=true apply -f app.yaml'
//                 }
//             }
//         }
//     }

//     post {
//         success {
//             echo 'Pipeline succeeded!'
//         }
//         failure {
//             echo 'Pipeline failed!'
//         }
//     }
// }

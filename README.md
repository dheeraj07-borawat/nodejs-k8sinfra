# nodejs-k8sinfra


docker build -t my-nodejs-app .
docker run -p 3001:3001 my-nodejs-app



docker tag my-nodejs-app vanshu07/my-nodejs-app:latest
docker push  vanshu07/my-nodejs-app
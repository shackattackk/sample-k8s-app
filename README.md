# sample-k8s-app

## Create Node App and Dockerize

- create a package.json file describing the app and its dependencies
- create an index.js file
- create a Dockerfile
  - Define image
    ```
    FROM node:14
    ```
  - Create app Directory to hold application code inside the image, this will be the working directory for the app
    ```
    WORKDIR /usr/src/app
    ```
  - Copy package.json file and install dependencies
    ```
    COPY package\*.json ./
    RUN npm install
    ```
  - Copy source code inside the docker image
    ```
    COPY . .
    ```
  - Map port and bind to port of app which is 8080
    ```
    EXPOSE 8080
    ```
  - Define command to run the app
    ```
    CMD ["node", "index.js"]
    ```
  - Create .dockerignore file, contents should include:
    - node_modules
    - npm-debug.log

## Build and run the image

- Go to the directory that has the Dockerfile and run the ff:
  ```
   docker build -t <image_tag> .
  ```
- Check if image has been built
  ```
   docker images
  ```
- Run the image, use the -d flag to run it in detached mode

  ```
  docker run -p 8000:8080 -d shackattackk/node-web-app
  ```

- Verify the running container
  ```
  docker ps
  ```
- Push docker image to docker hub
  ```
  docker push shackattackk/node-web-app
  ```

## Deploy to a kubernetes cluster using docker for desktop

- enable kubernetes in the docker for desktop settings. Settings > Kubernetes > Enable Kubernetes > Apply & restart
- create a yaml file containing the kubernets definitions. In this project, it is named 'web.yml'
  - Create two objects in the yaml file - deployment and nodeport service objects
- Deploy application to kubernetes
  ```
  kubectl apply -f web.yml
  ```
- Expected output after running the command above
  ```
  deployment.apps/node-demo created
  service/web-entrypoint created
  ```
- Verify if deployments and services are running
  ```
  kubectl get deployments
  NAME        READY   UP-TO-DATE   AVAILABLE   AGE
  node-demo   1/1     1            1           9s
  ```
  ```
  kubectl get services
  NAME             TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
  kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP          15m
  web-entrypoint   NodePort    10.100.180.142   <none>        8080:30001/TCP   8m37s
  ```
- Using a browser, visit localhost:30001 and verify if the application is accessible

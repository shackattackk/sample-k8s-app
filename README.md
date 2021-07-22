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

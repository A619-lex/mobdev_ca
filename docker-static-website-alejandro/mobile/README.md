Creating Ionic Project
Install npm and node using this link, if it’s not installed
Install ionic as a global dependency
npm install -g ionic
Create a new project using ionic start
ionic start myApp blank
Your Ionic Project is created. Now let’s move on to create a Dockerfile for our Ionic Project.

Creating Dockerfile
Go to Root Directory of the Project
cd myApp
Create a file named Dockerfile
touch Dockerfile
Open Dockerfile
vi Dockerfile
Put the following in Dockerfile (Press i to start inserting the text)
FROM node:13-alpine as build
WORKDIR /app
COPY package*.json /app/
RUN npm install -g ionic
RUN npm install
COPY ./ /app/
RUN npm run-script build:prod
FROM nginx:alpine
RUN rm -rf /usr/share/nginx/html/*
COPY --from=build /app/www/ /usr/share/nginx/html/
Save Dockerfile by pressing esc and shift+z twice
Voila! Your Dockerfile is ready to run.

Before you run it, let’s understand line by line that what we are actually doing in our Dockerfile.

FROM node:13-alpine as build
In the first line, we are telling Docker to use node13 alpine from dockerhub to use as a base image which will allow us to run the commands related to npm. Also, we are telling to give it a short name build which we can refer in our file further.

WORKDIR /app
COPY package*.json /app/
In the above lines, we are creating a working directory by name of app and copying package and package-lock.json to app folder.

RUN npm install -g ionic
RUN npm install
For ionic to run, we have to install ionic globally and then we are running npm install to install all the project dependencies.

COPY ./ /app/
In above line, we are copying everything from the root directory to app directory.

RUN npm run-script build:prod
The above command runs our custom script which is simply translation of ng build –prod. It creates a www folder of our project, which we will be using to deploy to nginx.

FROM nginx:alpine
RUN rm -rf /usr/share/nginx/html/*
COPY --from=build /app/www/ /usr/share/nginx/html/
In the above lines, we are first telling Docker to use the nginx from dockerhub. After that we are removing everything present in /usr/share/nginx/html folder and eventually copying everything from www folder to the html folder.

Now, when we have knowledge of what is happening in our Dockerfile, let’s move on to the part where we run our commands.

Build and Deploy using Dockerfile
To build a Docker Image, we have to run the following command in our terminal:

docker build -t myApp:v1
To run the built docker image, use the following command:

docker run -d --name myAppContainer --network host myApp:v1 
Pretty simple! Isn’t it?

Also, you can download a ready template from this link for kick-starting your Ionic Project.

Thanks!
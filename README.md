# Simle-App-on-GCP
Deploying a simple html application and Nginx, composed a Dockerfile for it and deployed all to Google Cloud Platform (GCP) Cloud Run

## Create index.html and nginx.conf file
- First created a directory called `nginxpractice` on my linux distro
- Created an index.html file using `nano index.html`
- Produce some html content and save file
- Create a `nginx.conf` in nginxpractice directory. In this file, include a server directory, assign the port you want the application to run locally. While location directive is declared within the server block. This provides requests for specific files an folders. using:
```
location  / { 
``` 
This means that you want the request to pick from the root folder, which is `nginxpractice` in this case. 
Save your `nginx.conf` file. You can check how my nginx.conf looks like in this main branch

## Compose a Dockerfile
- Create a Dockerfile in `nginxpractice` directory using `nano Dockerfile`
- Since we want to deploy our application using nginx, we will use nginx base image and we will build on that image.
- We will copy our `index.html` file into a directory /usr/share/nginx/html/index.html
Note: /usr/share/nginx/html/index.html directory is not located on your host machine. You are creating this directory in your docker container.
- Copy your `nginx.conf` file into /etc/nginx/conf.d/configfile.template
- then CMD 
- Save your Dockerfile.
You can check my Dockerfile file for clear explanation.

Confirm you can build an image from the Dockerfile created, use `docker build -t myapp:1.0 .`
if you are able to successfully build an image, this means your dockerfile has no error.

## Using Google Cloud Platform
- Create an account on GCP, you can use the free trial if you are just creating an account, create a GCP project and note your `PROJECT-ID`
- Go to Cloud Console > Dashboard > API & Services > Library
- Search and enable, Container Registry, Cloud Build and Cloud Run APIs
- Install Cloud SDK on your local machine using https://cloud.google.com/sdk/docs/install
- Login to GCP from your terminal and select your prefer project.
- You are expected to verify to confirm the right owner of the account is accessing the GCP account, paste your verification code in your terminal

## Build and Deploy on Cloud Run
- After successfully logging to GCP from your terminal, build your container using Cloud Build
```
gcloud builds submit --tag gcr.io/PROJECT_ID/service-name
```
- Confirm you have successfully built a container, go to Container Registry > Images on Cloud Console
- Now deploy the container to Cloud Run using
```
gcloud run deploy --image gcr.io/PROJECT_ID/service-name --platform managed --project=PROJECT_ID --zone=preferred_zone
```
- After deployment you can check your deployed application on Cloud Run
- Click on the service-name specified earlier, copy the url link and see your public on the internet. 

A view of how Cloud Build creates an image from your application
```
starting build "da0cc3c9-daf9-4549-8bc6-0ec52b489963"

FETCHSOURCE
Fetching storage object: gs://mynginx-project_cloudbuild/source/1614530221.008077-4c0f9e147ae247ac8cae0040488d3343.tgz#1614530223189466
Copying gs://mynginx-project_cloudbuild/source/1614530221.008077-4c0f9e147ae247ac8cae0040488d3343.tgz#1614530223189466...
/ [0 files][    0.0 B/  1.5 KiB]                                                
/ [1 files][  1.5 KiB/  1.5 KiB]                                                
Operation completed over 1 objects/1.5 KiB.                                      
BUILD
Already have image (with digest): gcr.io/cloud-builders/docker
Sending build context to Docker daemon  6.656kB

Step 1/4 : FROM nginx
latest: Pulling from library/nginx
45b42c59be33: Already exists
8acc495f1d91: Pulling fs layer
ec3bd7de90d7: Pulling fs layer
19e2441aeeab: Pulling fs layer
f5a38c5f8d4e: Pulling fs layer
83500d851118: Pulling fs layer
f5a38c5f8d4e: Waiting
83500d851118: Waiting
19e2441aeeab: Download complete
ec3bd7de90d7: Verifying Checksum
ec3bd7de90d7: Download complete
f5a38c5f8d4e: Verifying Checksum
f5a38c5f8d4e: Download complete
83500d851118: Verifying Checksum
83500d851118: Download complete
8acc495f1d91: Verifying Checksum
8acc495f1d91: Download complete
8acc495f1d91: Pull complete
ec3bd7de90d7: Pull complete
19e2441aeeab: Pull complete
f5a38c5f8d4e: Pull complete
83500d851118: Pull complete
Digest: sha256:f3693fe50d5b1df1ecd315d54813a77afd56b0245a404055a946574deb6b34fc
Status: Downloaded newer image for nginx:latest
 ---> 35c43ace9216
Step 2/4 : COPY index.html /usr/share/nginx/html/index.html
 ---> 579c4d50f76e
Step 3/4 : COPY nginx.conf /etc/nginx/conf.d/configfile.template
 ---> b7c0a0c39eaa
Step 4/4 : CMD sh -c "envsubst '\$PORT' < /etc/nginx/conf.d/configfile.template > /etc/nginx/conf.d/default.conf && nginx -g 'daemon off;'"
 ---> Running in d348413e8cf4
Removing intermediate container d348413e8cf4
 ---> ab76802e4e03
Successfully built ab76802e4e03
Successfully tagged gcr.io/mynginx-project/practice:latest
PUSH
Pushing gcr.io/mynginx-project/practice
The push refers to repository [gcr.io/mynginx-project/practice]
633000a62cb6: Preparing
8c9dd397c5e0: Preparing
2acf82036f38: Preparing
9f65d1d4c869: Preparing
0f804d36244d: Preparing
9b23c8e1e6f9: Preparing
ffd3d6313c9b: Preparing
9eb82f04c782: Preparing
9b23c8e1e6f9: Waiting
ffd3d6313c9b: Waiting
9eb82f04c782: Waiting
2acf82036f38: Layer already exists
0f804d36244d: Layer already exists
9f65d1d4c869: Layer already exists
9b23c8e1e6f9: Layer already exists
9eb82f04c782: Layer already exists
ffd3d6313c9b: Layer already exists
633000a62cb6: Pushed
8c9dd397c5e0: Pushed
latest: digest: sha256:9b944d2189232ca67b9149e3523ebd25c7607b3135a9016cd366bd74f80c5443 size: 1984
DONE
```









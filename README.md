# Deploying a Flask API

This is the project repo for the fourth course in the [Udacity Full Stack Nanodegree](https://www.udacity.com/course/full-stack-web-developer-nanodegree--nd004): Server Deployment, Containerization, and Testing.

In this project i have containerized and deployed a Flask API to a Kubernetes cluster using Docker, AWS EKS, CodePipeline, and CodeBuild.

The Flask app that will be used for this project consists of a simple API with three endpoints:

- `GET '/'`: This is a simple health check, which returns the response 'Healthy'. 
- `POST '/auth'`: This takes a email and password as json arguments and returns a JWT based on a custom secret.
- `GET '/contents'`: This requires a valid JWT, and returns the un-encrpyted contents of that token. 

The app relies on a secret set as the environment variable `JWT_SECRET` to produce a JWT. The built-in Flask server is adequate for local development, but not production, so have used the production-ready [Gunicorn](https://gunicorn.org/) server when deploying the app.

## Initial setup
1. Fork this project to your Github account.
2. Locally clone your forked version to run the project on local machine.

## Dependencies

- Docker Engine
    - Installation instructions for all OSes can be found [here](https://docs.docker.com/install/).
    - For Mac users, if you have no previous Docker Toolbox installation, you can install Docker Desktop for Mac. If you already have a Docker Toolbox installation, please read [this](https://docs.docker.com/docker-for-mac/docker-toolbox/) before installing.

     
## Project Steps

The following steps have been completed as part of this project,

1. Writing a Dockerfile for a simple Flask API
2. Building and testing the container locally
3. Creating an EKS cluster
4. Storing a secret using AWS Parameter Store
5. Creating a CodePipeline pipeline triggered by GitHub checkins
6. Creating a CodeBuild stage which will build, test, and deploy your code

## Local Setup

### Follow the steps to run the app locally using docker.

Run the commands from the root directory of the project where the Dockerfile is placed.

```bash
docker build -t <image_name> .
docker image ls
docker run --name <container_name> --env-file=.env_file -p 80:8080 myimage
docker ps
```
>_note_: localhost:8080 is the application url.

Save the below content in .env_file 
```bash
JWT_SECRET='myjwtsecret'
LOG_LEVEL=DEBUG
```

### Clean up after use
```bash
docker container stop <container_id>
docker container rm <container_id>
docker image rm <image_id>
```

## AWS EKS

The same project has been deployed to a Kubernetes cluster using Docker, AWS EKS, CodePipeline, and CodeBuild. 

### Appplication url running in EKS cluster

```bash
aa9f31d276af04b86a580db30f142196-2078937600.us-east-2.elb.amazonaws.com
```

## How to test the application both running locally as well in cloud

```bash
export TOKEN=`curl --data '{"email":"<EMAIL-ID>","password":"<PASSWORD>"}' --header "Content-Type: application/json" -X POST <APP_URL>/auth  | jq -r '.token'`

curl --request GET '<APP_URL>/contents' -H "Authorization: Bearer ${TOKEN}" | jq .
```

## Authors
Yours truly, Sourabi Kannan 

## Acknowledgements 
The instructors, mentors, fellow students and entire team at Udacity.






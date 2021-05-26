# Cloud Run Express App

In this repo I have the following
- A simple express server, with 2 routes and some simple testing with chai. 
  - GET `/` for a heath check route
  - GET `/hello` for a simple hello world return. This can be used to check the app routing works when deployed.
- Dockerfile to containerise the app.
- Cloud build yaml file for building the image, publishing to GCR and then deploying to Cloud Run.

This app is a demonstration of the code required for a simple serverless set up, complete with a CI/CD chain. 

## App Developer

To test the app, run `npm test`

To start the app run `npm start`


To build a docker image with docker installed `docker build . -t <tag-name>`

To run the dockerised app locally `docker run -p <exposed-port>:8080 -d <tag-name>` where the exposed port is where you would like it to run e.g `localhost:<exposed-port>`


## Cloud Developer

In order to deploy the app, there are some functionality used from GCP. The first is triggers, this is what will ensure that when you push the code to the repo, the app will also get deployed to cloud run. 

### Setting up triggers for cloud build 

This can done via the console (under cloud build > triggers) or command line with an appropriate trigger.json file. The command line to deploy the trigger is:
```bash
PROJECT=<project-name>
curl -X POST \
    https://cloudbuild.googleapis.com/v1/projects/${PROJECT}/triggers \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer $(gcloud config config-helper --format='value(credential.access_token)')" \
    --data-binary @trigger.json
```

### Cloud build

When the trigger detects a change to a branch, it will call the cloudbuild file. In this example we deploy the app to cloud run, but we could also deploy to kubernetes if desired.



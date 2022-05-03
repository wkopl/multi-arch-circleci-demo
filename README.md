# Multi-Arch Docker CircleCI Example
[![CircleCI Build Status](https://circleci.com/gh/james-crowley/multi-arch-circleci-demo.svg?style=shield)](https://circleci.com/gh/james-crowley/multi-arch-circleci-demo) [![Software License](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/james-crowley/multi-arch-circleci-demo/main/LICENSE) [![Docker Pulls](https://img.shields.io/docker/pulls/jimcrowley/multi-arch-circleci-demo)](https://hub.docker.com/r/jimcrowley/multi-arch-circleci-demo)


With the release of CircleCI's Arm executor, we can now build on multiple architectures via CircleCI's Cloud offering. 
Building on architecture offers better performance/speed and the assurance that your application will indeed work on your platform.

While some examples show using Docker Buildx with QEMU, this can lead to issues with low level languages looking like
they work, but in reality they fail when deployed on your platform.  

For this example we will build a Python application, build Docker images for each architecture(`arm64`, `amd64`, `s390x`, and `ppc64le`), 
and construct a manifest to tie the Docker images together into a single tag. 

## CI/CD Setup

For this example we will be using a mix of CircleCI infrastructure and self-hosted runners to build, test and push Docker images. The two main executors
we will be using is the machine executor for both `amd64` and `arm64`. Using CircleCI's self-hosted runners, we are able to test `s390x` and `ppc64le` by installing
the agent onto a LinuxOne and Power server. 

We want to setup CircleCI to accomplish the following:

- Trigger on every commit to `main` and every PR on this repo
- Build the Flask Demo container
- Test the container to make sure it is functioning
- If tests past, tag the images and push the architecture specific images
- Create Docker manifests to allow users to pull down the image without caring about architecture
- Deploy the new images to their appropriate platforms for http://multi-arch.circleci-demo-app.com


Currently, the demo deploys a Flask based website utilizing Docker. The flask server is load balanced on multiple architectures! Feel free to refresh a couple times and your architecture should change! You can view the live site [here](http://multi-arch.circleci-demo-app.com).

Some other links that might be important:

- [DockerHub Link](https://hub.docker.com/r/jimcrowley/multi-arch-circleci-demo)
- [SonarQube Scan](https://sonarcloud.io/project/configuration?id=james-crowley_circleci-demo-app)
- [Snyk Scan](https://app.snyk.io/org/james-crowley/project/6cf9bd21-9738-4915-a9b6-9936fc3f8140)


## Deployment Documentation

### Python/Flask Application

Project structure:

```
.
├── .ci                         - Docker Build Tools
├── demo
|    ├── app/                   - Python Application
|    ├── requirements.txt
|    └── flask_run.py
|    └── config.py
├── Dockerfile                  - Flask Application Dockerfile
```

## Deploy with Docker

```
$ docker run -d -p 8080:8080 jimcrowley/multi-arch-circleci-demo:latest 
```

After the application starts, navigate to `http://localhost:8080` in your web browser.

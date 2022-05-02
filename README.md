# Multi-Arch Docker CircleCI Example

With the release of CircleCI's Arm executor, we can now build on multiple architectures via CircleCI's Cloud offering. 
Building on architecture offers better performance/speed and the assurance that your application will indeed work on your platform.

While some examples show using Docker Buildx with QEMU, this can lead to issues with low level languages looking like
they work, but in reality they fail when deployed on your platform.  

For this example we will build a Python application, build Docker images for each architecture(`arm64`, `amd64`, and `ppc64le`), 
and construct a manifest to tie the Docker images together into a single tag. 

## CI Setup

For this example we will be using a mix of CircleCI infrastructure and self-hosted runners to build, test and push Docker images. The two main executors
we will be using is the machine executor for both `amd64` and `arm64`. Using CircleCI's self-hosted runners, we are able to test `ppc64le` by installing
the agent onto a PowerPC server. 

We want to setup CircleCI to accomplish the following:

- Trigger on every commit to `main` and every PR on this repo
- Build the Flask Demo container
- Test the container to make sure it is functioning
- If tests past, tag the images and push the architecture specific images
- Create Docker manifests to allow users to pull down the image without caring about architecture
- Deploy the new images to their appropriate cloud platforms


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

## Deploy with Docker

```
$ docker run -d -p 8080:8080 jimcrowley/multi-arch-circleci-demo:latest 
```

After the application starts, navigate to `http://localhost:8080` in your web browser.

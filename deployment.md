# Hydro Software Deployment and Automation

This semester I developed the new version of Olin Hydroponics Club's automation architecture. For more information on the design see [quality.md](quality.md). This software was developed to be maintainable, standardized, and easy to deploy. As an offshoot of this architecture I also built a simple image API that stores and serves images of our plants. This storage API was written to be easy to deploy and update.

Below are some descriptions of the systems used to assit with deployment and maintainability.

## Git Hooks
We created a centralized repository for git hooks in our github organization. This repository was intended to act as a centralized location to store organization wide hooks that should be used in every repository. When a new repository is created, a local hard link should be created from the local copy of the hooks github repository to a /hooks folder in the new repostory. This keeps the hooks centralized, and makes it easier to update them in one place later on. This also allows the hooks to be checked into github and their versions tracked. A full setup guide can be found [here](https://docs.google.com/document/d/1-xlYPpF-vdUu03zkPbF8pHdN6CtZb2e-jaxFh9VxUsk/edit?usp=sharing). 


## Continuous Integration
When code is merged to Mother Nature main branch, two github workflows are triggered. 

The first github workflow builds the code, runs [go vet](https://pkg.go.dev/cmd/vet) to check for errors, and runs [golangci-lint](https://github.com/golangci/golangci-lint) to check the code style. This github workflow can be found [here](https://github.com/Olin-Hydro/mother-nature/blob/main/.github/workflows/build-lint.yaml). This github workflow creates a standard styling guideline through golangci-lint, and allows any contributor to get quick feedback on the latest build after pushing.

The second github workflow runs the test suite on the latest version of the code. This workflow can be found [here](https://github.com/Olin-Hydro/mother-nature/blob/main/.github/workflows/test.yaml). This workflow allows developers to get immediate feedback on the functionality of their code upon pushing to the remote repository.

## Configuring Cloud Machines
Before deploying software, we needed somewhere to deploy it to. We used hetzner instead of AWS for moral and financial reasons. After creating a basic cloud server, the main setup that we needed to perform was to adjust the security settings. We reduced the security to allow any traffic on port 80 over http to access the server, setting the stage for our image API.

## Automate Deployment
I simplified the deployment process for the image storage API ([link](https://github.com/jonaskaz/plant-analysis/tree/main/storage_api)) using Docker and Ansible. The API is a simple FastAPI app that takes images and stores them locally, making them available via GET request by timestamp. Docker was used to create a standard container that can reliably run the FastAPI app [link](https://github.com/jonaskaz/plant-analysis/blob/main/storage_api/Dockerfile). This docker image is also uploaded to a dockerhub repository so that the remote server can pull it for deployment. Next, an ansible playbook was created to set up the remote server. The ansible playbook connects to the host, installs relevant packages, pulls the latest docker image for the API and runs it. The playbook can be found [here](https://github.com/jonaskaz/plant-analysis/blob/main/storage_api/ansible). This playbook makes the API portable to many cloud servers and providers, as it can be deployed by simply providing a new IP address host.

## Conclustion
The above implementations demonstrate my ability to write git hooks, automate testing and deployment, and deploy software on cloud infrastructure. 

# Hydro Software Deployment and Automation

This semester I deployed starter version of Olin Hydroponics Club's API. More information on the API [here](https://github.com/tzou2024/hydrangea).


Below are some descriptions of the systems used to assit with deployment and maintainability.


## Continuous Integration

When code is pushed to the Hydrangrea main branch, 3 github workflows are triggered. 

This first github workflow is a [linter](https://github.com/py-actions/flake8) that installs the Python flake8 package and excesutes flake8 on python files. To check for linting errors. The second gitub workflow is a [formatter](rickstaa/action-black@v1) to check the formatting the python code. These two workflows makes a standard styling guideline for all integrated python code that is pushed. The third github workflow the unit tests made for each route using pytest. This means that developer can get immediate feedback on the functionality of the code once it has been pushed to the remote repository.

Links to workflows:

[Linting](https://github.com/tzou2024/hydrangea/blob/main/.github/workflows/lint.yml)

[Autoformatting](https://github.com/tzou2024/hydrangea/blob/main/.github/workflows/black.yml)

[Testing](https://github.com/tzou2024/hydrangea/blob/main/.github/workflows/testing.yml)

## Dockerization
I also containerized the API using Docker. I used docker to create a standard container that can run the FastAPI app. I build a Docker image using a [Dockerfile](https://github.com/tzou2024/hydrangea/blob/main/Dockerfile). I also pushed the image to [dockerhub](https://hub.docker.com/r/tzou2024/hydroapi) so that anyone could pull and run it. 

## Configuring Cloud Machines
I deployed my app using AWS Elastic Beanstalk, a service that serves as an abstraction layer on top of ECS. Elastic Beanstalk automatically handles the deployment—from capacity provisioning, load balancing, and auto scaling to application health monitoring. I used a EC2 T2.micro instance. I also seperately set up a mongoDB atlas database so I don't store the database locally. You can acess my deployment [here](http://hydroapi-env.eba-p7wttzu3.us-east-1.elasticbeanstalk.com/docs). Elastic Beanstalk uses docker-compose to pull and run the image, so I also setup a [docker-compose.yml](https://github.com/tzou2024/hydrangea/blob/main/docker-compose.yml) file.

## Automate Deployment
I automated the deployment process using github actions. My workflow only redeploys the app on pushes that are tagged as releases using github tags, rather than redeploying everytime there is a push to main. The action builds the docker image, utlizing github secrets for the environemnt variables, zips up a deployment package, and deploys it to elastic beanstalk with a timestamp version label in EB.

[deployment workflow](https://github.com/tzou2024/hydrangea/blob/main/.github/workflows/docker.yml)

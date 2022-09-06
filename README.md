# heycar Infrastructure Engineer take home test

## Air Quality API

You are on a venture team that is building a business in the environmental sector. As part of this effort, some engineers have built a small HTTP API in Golang that allows consumers to discover the air quality in their local area. The API receives latitude and longitude coordinates via a HTTP GET request, and subsequently retrieves air quality data for those coordinates by making a further HTTP request to the extneral [Air Visual API](https://airvisual.com/api) . Once the Air Visual response is received, our API returns a JSON response containing the nearest City to the requested coordindates along with the latest air quality index in that location. 

- Air quality URL: `/air-quality?lat=50&lon=1`

To aid with analytics and future evolution of the API, each incoming query is stored in a PostreSQL database, and a second external HTTP GET endpoint exposes all the recently queried cities.

- Queried cities URL: `/queried-cities`

As the venture continue and the business evolves, the API's responsibilities will inevitably change. It is also unknown what the future levels of traffic for the API could be, as this is dependent on the success of the venture.

# The Challenge - Part 1

The team have requested your assistance and expertise to help deploy the Air Quality API and associated database to Amazon Web Services. 

- Automate deployment of the Air Quality API to AWS
- You may use any tools or services you choose
- Make any code changes you feel are necessary to facilitate the deployment

Challenge tips:
- Think about how your solution will support future evolution and changes to the API
- We value simple solutions over anything complex. Don't overengineer
- This doesn't need to be production-ready code, but be prepared to explain what further changes you'd make if you had more time

AWS Access:
- The [AWS Free Tier](https://aws.amazon.com/free/) should provide you with enough free usage to complete this task. However, if you would like to use any services that fall outside the free tier, then we'll happily reimburse you for this usage (providing it's fair and you haven't just been mining Bitcoin)

# The Challenge - Part 2

The team have also requested your advice for their ongoing development and ownership of the Air Quality API.

Put together a short presentation that details:

- A proposed Continuous Delivery pipeline for the Air Quality API
- Changes that should be made to the API to facilitate its operation in production

Challenge tips:
- Consider security, resiliency, monitoring etc


## Initial Setup
- Install Docker locally from [https://docs.docker.com/install/](https://docs.docker.com/install/)
- Run `start.sh` to start the API
- Visit [http://localhost:5000](http://localhost:5000) in a browser to confirm the API is running
- Run `test.sh` to execute tests

# Changes made _(to the original code repository; .zip)
- Added a .travis.yml file. So whenever there is change implemented on GitHub it will trigger job on Travis which will build image and push to Docker Hub therefater image will be deployed on AWS.
- Added a Dokerrun.aws.json file. Describes how to deploy a remote Docker image as an Elastic Beanstalk application. This JSON file is specific to Elastic Beanstalk. If your application runs on an image that is available in a hosted repository, you can specify the image in a Dockerrun.
- Edited docker file under api folder , failing to build image with error "go: go.mod file not found in current directory" so added an extra line "RUN go mod init" above "RUN go get -d -v -t ./..." which has fixed the issue.

# Response
- Install docker locally and successfully run api on port 5000 also performed test as mentioned and it passed.

## Challenge - Part 1
![App Auto Deployment](https://user-images.githubusercontent.com/63913243/130072826-c62b9864-5bd0-45a8-bd6b-ce328c7a9237.PNG)

- Push our code into github repository then Travis CI will take that code using .travis.yml, build a docker  an image and push that to dockerhub which inturn deloyed to AWS through deployment part of .travis.yml and Dockerrrun.aws.json file.

![Travis Environment variables](https://user-images.githubusercontent.com/63913243/129980477-80904ccb-cd19-4f75-b895-18a86ce763a9.png)

- Travis will have credential for Dockerhub and Elastic Beanstalk(AWS) to communicate with them.

![AWS flow for deployment](https://user-images.githubusercontent.com/63913243/130073607-1ab78a7e-189b-435e-a3d1-9498b2996caf.PNG)

- Travis will talk Elastic BeanStalk on AWS , it is Paas service here docker in use so deploy container on ECS.

- It uses a file called Dockerrun.aws.json to start container which is received from github through travis.

## Challenge - Part 2
![Continous Delivery](https://user-images.githubusercontent.com/63913243/130123276-fee82e43-f81e-4314-9464-4fe32f22b3b4.png)

* `Flow Proxy` - The goal of the [Docker Flow Proxy project](https://github.com/vfarcic/docker-flow-proxy) is to provide an easy way to reconfigure proxy every time a new service is deployed, or when a service is scaled. It does not try to "reinvent the wheel", but to leverage the existing leaders and combine them through an easy to use integration. It uses HAProxy as a proxy and adds custom logic that allows on-demand reconfiguration.
* `Flow Swarm Listener` - The goal of the [Docker Flow Swarm Listener project](https://github.com/vfarcic/docker-flow-swarm-listener) is to listen to Docker Swarm events and send requests when a change occurs. At the moment, the only supported option is to send a notification when a new service is created, or an existing service was removed from the cluster.
* `Sonatype Nexus Repository Manager 3` - Based on CentOS, a free [binary repository manager](https://github.com/sonatype/docker-nexus3) with universal support for popular repository formats such as maven, yum, raw, docker and many other
* `SonarQube` - [SonarQube](https://github.com/SonarSource/sonarqube) provides the capability to not only show health of an application but also to highlight issues newly introduced. With a Quality Gate in place, you can fix the leak and therefore improve code quality systematically
* `Jenkins` - As an extensible automation server, [Jenkins](https://hub.docker.com/r/jenkinsci/blueocean/) can be used as a simple CI server or turned into the continuous delivery hub for any project
* `GitLab Community Edition (CE)` -  [Gitlab](https://github.com/sameersbn/docker-gitlab) is an open source end-to-end software development platform with built-in version control, issue tracking, code review, CI/CD, and more. Self-host GitLab CE on your own servers

| Arch Principles | Hardware | Software |
| --------------- | -------- | -------- |
| Security | The custom Linux distribution _(Alpine based)_ has very small attach surface. Secondly, all ports on ELB are closed by default. Thirdly, no direct SSH access to worker nodes. The custom Linux distribution used by Docker for AWS is carefully developed and configured to run Docker well. Everything from the kernel configuration to the networking stack is customized to make it a favorable place to run Docker. | Only front-end services publish ports _(e.g. 80 or 443)_ via ELB |
| Resiliency | The lifecycle of nodes is managed using auto-scaling groups, so that if a node enters an unhealthy state for unforeseen reasons, the node is taken out of load balancer rotation and/or replaced automatically and all of its container tasks are rescheduled | If a node is replaced, all of its container tasks are rescheduled automatically |
| Monitoring | An option could be Elastic Stack (including Metricbeat on each node to ship system stats to Elasticsearch) |  An option could be Elastic Stack (including Metricbeat on each node to ship docker container stats to Elasticsearch) |

Testing done
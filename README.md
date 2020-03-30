# Why Cloud-Native Development?

At the beginning of 2019 I joined Deloitte from Makers Academy. Makers Academy is a 12 week coding bootcamp which aims to output a language agnostic developer through their focus on techniques and processes over languages and frameworks. While this set me up in the developer scope quite nicely, my first project was to fall under the cloud native development approach and therefore developing code was only part of the story.

When first researching cloud native development I found I could open 10 different webpages, or speak to 10 different people and the responses would never be consistent. That being said, they would all have a common thread, they were a step away from the traditional articles you read about cloud computing advantages and would focus on three key aspects; containers, microservices and automation (including container orchestration). This is because cloud native development isn't synonymous to the concept of the cloud. Instead, it is an extension, a foundation to success in the cloud.

## Our decision to follow a cloud-native approach

There are many different reasons you might make the decision to implement a cloud-native approach. But as the name suggests, the decision should be made from the start, as it is something that should drive your architectural and infrastructural designs.

What were the goals of our project which made cloud-native a sensible design choice?

#### 1. Use of the cloud
  * Cost efficiency
    * OpEx over CapEx
    * Scaling to fit needs rather than guessing
  * Flexibility
    * Choice of tools
    * Scalability (as above)
    * Professional hardening options
  * Disaster recovery
    * Helps deal with mitigating against downtime as well as the less likely but still possible emergency scenarios such as natural disaster
  * Consistency
    * Automation removing human error

#### 2. Speed and agility in development - in both services and delivery
  * A loosely-coupled architecture
    * The ability to break down the application into smaller, self-isolated systems, thus removing unnecessary dependencies across the application
    * Flexibility in stack for each service - use the best suited tools for the task in hand
  * Constant interaction with the client to enable a fast feedback loop - utilise a DevOps focused continuous integration and continuous delivery pipeline for each section
    * This relationship between software development and DevOps caters to an iterative approach where design, build, test and deploy happen rapidly, frequently and more consistently

#### 3. Packaging the code for a client
  * Reusable APIs - what if they decided they also wanted a mobile app?
    * Same services, different wrapper
  * Infrastructure as code - consistency in deployment and maintenance of the application as a whole

These aspects shaped the way we wanted to approach the application, we wanted to take advantage of new technologies and more specifically, the cloud. This meant stepping away from a tried and tested monolithic development approach and moving to containers, microservices and automation. It is important to understand the relationship between these three concepts and how they can direct your delivery. It is the use of these technologies and processes, along with increased visibility across the whole development lifecycle that defines cloud-native - it's not just about being hosted on the cloud. By following these principles you can reduce deployment risks while increasing scalability as well as speed and agility in development.

## The foundations of cloud-native development

### Microservices
At it's heart, this is an architectural approach to splitting your services. One of the major pattern shifts comes through the relationship between services and databases. Each service should have its own database, and while this can lead to duplication of data when used in an enterprise scale, it allows for a true loosely-coupled application. This separation and removal of dependencies has multiple benefits, for instance the isolation in development allows constant improvements to be made, rather than the worry of a seemingly small change having cascading effects. Huge, monolithic, codebases can reduce productivity for this exact reason, and by splitting the code it empowers the developer to deploy to the environment more frequently, and thus catch errors earlier.

As each service is independent of one another in a microservices architecture, there are no restraints when designing each part of the application. You have a full choice of stack per service, meaning you can get the best out of each element. For instance, we used Python for the data science service, Node.JS for the data access layer. This abstraction from one another also increases the ability for independent controls for each service in the cloud (we will come back to this in the automation section).

When talking about microservices you will often get the term Application Programming Interface, or API, thrown in for free. And I am doing no different, we supported the use of declarative APIs through RESTful HTTP to allow our services to communicate seamlessly whilst still being decoupled. This opened up the need for a single entry point for the front end to interact with multiple microservices. Kevin Hoffman, in his book Beyond the Twelve-Factor App, adds 3 key factors to his original 12 explaining how to deploy a portable and resilient web app:
* API First - all code in back end services will be consumed by a front-end client, gateway, or another service
* Telemetry -	there is a large drop off in visibility when migrating from local to the cloud, so there must be stringent tracking and monitoring
* Authentication/Authorization	- seems like an obvious addition, but all services should know who is making the request and whether their role allows them to do so

We choice to adopt AWS API Gateway Integrated with Cognito. While the client had its own Gateway in place, we chose this famous duo for our development cycle as it breaks down two often difficult but necessary aspects of integrating microservices:
  * Creating a single, unified entry point to the microservices behind it
  * Implementing security - authentication and authorisation of users

The inclusion of an API Gateway controls the interaction of the front end with all the back end services and removes a lot of cross cutting concerns. This lets the focus be on each of the services independently, while mimicking a monolithic application through the single entry point. The abstraction of the authentication and authorisation of users to the API gateway removes repeated code across the microservices and prevents the service being called unnecessarily while securing the APIs. In addition to this, it has a pretty extensive monitoring set up to add to the vast watches automation brings to the table.

***Disclaimer:*** *The use of AWS API Gateway and Cognito was new to me. When researching it there were a lot of guides for big scale implementation, but as a developer I just wanted a sandbox to play in when starting out. I therefore worked out the most basic implementation of them and created a step through guide so you can do the same: __________________________________________. Note that this guide is just to get it up and running so you can adapt it to fit your use case and if not a full guide on either service.*

### Containers
This is in some ways the bridge between microservices and automation. In the most basic sense, you can think of a container as a lightweight virtual machine. Unlike virtual machines, containers run on the host's operating system, meaning each container doesn't need to include. This increases start-up speed and reduces required memory. This allows for them to be more trim, but also increases portability, meaning it is simple to spin them up pretty much anywhere, from different operating systems on your local machine to a remote server in the cloud.

Not only is it nice to have the flexibility to host containers either locally or on virtual machines, but it also gives the developer a realistic sandbox to play in - gone are the days you'd hear: "hmm, well it works fine on my machine!". The environment is predictable and consistent meaning the dev and test environments will slide into production with less management and debugging. This tries to redefine the relationship between developers and operations, moving towards a more collaborative interaction. By increasing the developers visibility of the environment that the production application will deploy to, it removes the worry of bugs appearing only when transferred to the cloud.  

Containerisation, by including all the code and dependencies of the application, reduces the need for independent and complex deployment processes. Instead it becomes as simple as "run this container". This, along with the behavioural consistency between development and production environments allows CI/CD pipelines to be more simplistic and effective. This incorporates the aforementioned benefits of speed and agility in development through a quick feedback loop. This mitigates against configuration and integration issues and increases productivity of development by reallocating time to pushing features. The separation of services into containers supports the microservice architecture assisting in this quick development lifecycle.

In terms of security, there is a great case for improvement through true isolation. If a service only interacts with itself then any attack on the other containers will have no impact. However, the issue arises when they have a common element, this is usually seen through the shared OS or shared application layer opening the door to attacks. This is mitigated against by using namespaces to limit access to certain resources, but is definitely food for thought.    

Containers with CI/CD pipelines have a very marketable existence but it becomes an even more powerful tool when integrated with container orchestration. It is this aspect which makes it fundamental to a cloud-native development cycle.

### Automation
Automation has been an important part of software development for a while, with the need for CI/CD pipelines to support rapid deployment. As mentioned earlier, microservices and containers has led to much less time between deployments, which means less time for testing and less time for quality assurance. Having this speed without decreasing reliability is fundamental to a cloud-native approach - it completes the feedback loop from developer to internal business team and/or client. This relies on the ability to rollout improvements, or perform updates without waiting for an arbitrary release date and without enduring downtime. Automation is able to combat downtime risk to ensure continuous integration by using a blue/green deployment strategy. Blue/green development involves two identical environments, one of them (lets say green) accepts the live traffic while the other (lets say blue) is spun up with the changes and tested. Once the tests and health checks are done the traffic is moved over to the blue environment and the green is terminated. By automating this pipeline it is quick to identify and correct defects, or refuse the delivery with minimal human intervention.

There are a few issues with attempting to manually build this sort of infrastructure every time. First and foremost, it would fail to deliver at the speed required. Secondly, with every manual deployment there is a potential for discrepancies which will lead to a unique environment for that individual deployment - this is referred to as a snowflake. These days, with containerisation, standardising environments and virtual servers, there is no need for deployment discrepancies and has led to a practice called infrastructure as code (most commonly seen through Terraforming). This is where you code your infrastructure through templates to be able to rapidly deliver stable environments. These templates scripted in a way that is human readable and machine consumable. This gives a developer the ability to spin up identical environments each time at the click of a button (or a push to a branch). It is fast and reliable.

The idea of automation, including infrastructure as code, has been adopted through container orchestration and subsequent managed operators such as Kubernetes. By defining the desired state of your cluster, Kubernetes manages the lifecycle of your containers. This includes provisioning through rolling out new versions of the app and rolling back if there is an issue, networking, health checks, logging and scaling. When using containers running stateless microservices the ease to scale horizontally gives not only performance advantages but increases cost efficiency.

The speed of reaction in terms of deployment, failover and scaling (to name a few) is vital in cloud computing and the use of automation to assist these managed systems has changed the game.

## Some advantages of this approach
The above sections outline the foundations of a cloud-native development approach. It is evident when going talking about microservices, containers and automation that their key advantages overlap, if not enhance one another. By implementing these three principles you can create something that is:
  * Speed to market - isolation of services, breaking down barriers between development and deployment and efficient pipelines together form a formidable trio
  * Able to respond to change - quick releases, quick feedback, quick improvements
  * Reusable sections - each service is designed specifically for its role
  * Cost-efficient - utilises the cost benefits of the cloud but also increases the efficiency in development and provisioning
  * Reliable - infrastructure as code
  * Resilient - built for the unpredictable through the of orchestration of containers.. spikes, lulls and downtime are all handled automatically
  * Able to isolate failure - when hosting on the cloud it is inevitable there will failures, with the microservice architecture the failure is contained rather than having cascading affects

These benefits of cloud-native are without a doubt appealing. While these are not individually unique to this approach, when they are all implemented together they are strengthened.

However, even if you look at just one section of the design decision and focus on a microservice vs monolithic architecture, it is very use case specific. There is a trade off of simplicity, you get flexibility, reliability through isolation and ease of scalability but you lose out by also increasing complexity through distributed systems (including the testing), adding cross-cutting concerns and the potential of making bad cutting decisions. This is not a decision to make light of.

As we have seen through this post, migrating to cloud-native does not simply mean wrapping up the monolithic, legacy code and placing it in the cloud. There is a strong emphasis on microservices, containers and automation as well as increasing visibility during the lifecycle through collaboration. Through this you achieve reduced deployment risks while increasing scalability as well as speed and agility in development. This is how it helped us be successful but it is not one size fits all.

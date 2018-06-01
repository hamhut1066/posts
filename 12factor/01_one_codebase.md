I recently wrote a post on configuration management (link to cfg mgmt here). It got me thinking about writing a series of posts about what is meant when we talk about a 'cloud native' application. So This is the beginning of a series on writing, from scratch a cloud-native application.

I am choosing a simple application so that the focus is more on the principles.

This will be an API that allows one to find out information about SuperHeroes. I pulled the data down from [Keggle](https://www.kaggle.com/claudiodavi/superhero-set).

Using this project I will walk through how to Declare your dependencies in a reliable manner, how to setup and a CI/CD pipeline using (Gitlab andAWS lambda). I touch briefly on how to handle state in a distributed fashion.

I will also try and comment on how relevant the original manifesto's agendas are in todays environment 7 years on. Note that I will not be going through each of the Factors in the same order as defined in the manifesto as it does not allow me to incrementally build upon the project.

## Codebase

Fundamentally the rule is to keep one each codebase in a separate repository. This has a few advantages which become more obvious when applied to microservices. We want to have well-defined boundaries between services. This ties in with _Backing Services_ (post ~4) of treating all dependencies as 3rd-party. A single codebase per service allows for much finer-grained control of each service, such as releasing security-patches without having to notify or synchronise with dependent teams and services.

Not everyone agrees with this, in fact Google uses a huge Monorepo. This should be considered an anomoly, Not everyone is Google.

I am putting the csv files into the Repo and generating a sample project ready to be used with AWS lambda. You can find the initial version on my Gitlab: https://gitlab.com/hamhut1066/superhero-query/tags/codebase.

I have, for simplicity, decided to use [Chalice](https://github.com/aws/chalice) to build the project. This should significantly ease the process of interaction with AWS. To get us started, we can install _chalice_ and initialise a new repository:

```
pip install chalice
chalice new-project superhero-query
```

We can then verify that we can deploy our project using `chalice deploy`. We will first need to configure the AWS cli. The official documentation is pretty decent on how to do that https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html.

Now we can navigate to the supplied url and view our deployed code:

```
$ curl https://rnbn7cr4lb.execute-api.eu-west-1.amazonaws.com/api/ 
{"hello": "world"}
```

<!-- wrapup -->
I have already added a few dependencies to the project, and will start going through the code and what is going on in the next blog.

Join me next time as I jump into Dependency management and Isolation.


<!--
- start with an introduction to the motivations behind 12-factor applications
- Talk about how We are going to work through an example of setting up an application for 12-factor development (gitlab)
- Talk about how relevant the 12 factors are nowadays
- Use a motivating example  (an excuse for AWS lambda?...)
- 


-------------
I. Codebase
One codebase tracked in revision control, many deploys
II. Dependencies
Explicitly declare and isolate dependencies
III. Config
Store config in the environment
IV. Backing services
Treat backing services as attached resources
V. Build, release, run
Strictly separate build and run stages
VI. Processes
Execute the app as one or more stateless processes
VII. Port binding
Export services via port binding
VIII. Concurrency
Scale out via the process model
IX. Disposability
Maximize robustness with fast startup and graceful shutdown
X. Dev/prod parity
Keep development, staging, and production as similar as possible
XI. Logs
Treat logs as event streams
XII. Admin processes
Run admin/management tasks as one-off processes

-->

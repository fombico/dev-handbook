---
title: "CI/CD Pipeline"
date: 2020-03-26T16:18:50-04:00
draft: true
---

In this page we discuss about CI/CD (Continuous Integration / Continuous Delivery) Pipelines, which can be also be 
thought of as Build and Deployment Pipelines.

### What is a CI/CD Pipeline?

A pipeline is an automated sequence of steps to we want to run when we make changes to our code. Usually this involves:
- ensuring it compiles
- ensuring tests pass
- packaging it up
- uploading it somewhere
- downloading it
- deploying it

### Why should I have a CI/CD Pipeline?

In our industry, we want to get to prod often - to test new features, to fix bugs, to gather feedback, to iterate and 
build a better product. Given how important this is and how many steps can be involved, it's easy to miss a step - not
to mention if you rely on that one expert that happens to be on vacation. As such, it's best to automate these steps as
much as possible to make it repeatable.

### Why should my team be in charge of this?

Our teams know our code the best, what configuration we need, what external services we use. We want that level of
ownership from development to prod. We want to be able to choose whatever we decide is the best technology is to solve
the problem - and that includes our deployment process. Of course, in reality we'll hit various roadblocks - but we
we should push for having as much control of our own destiny as possible. This could mean having admin access, or at
least being able to install what plugins you want, or perhaps asking for a dedicated DevOps member in your team.

### What CI tools are available?

There are a number of CI tools available:
- Jenkins
- Concourse CI
- CircleCI
- Travis CI
- BitBucket CI
- GitLab CI/CD
- and more

Some are hosted, others are on-premise, or both.

---

Now let's take a deeper look into what constitutes a pipeline. It helps to think of it as two parts - a __build__ phase
and a __deploy__ phase, as the two have different goals. 
The output of the __build__ phase is a reusable artifact that you can deploy any time. 
The output of the __deploy__ phase is your code in an environment. 
You don't always want to immediately deploy the last thing you've built - for example to keep an environment stable.
You also don't want to build every time you want to deploy - this introduces extra time and variability to the process.

## Build Phase

![Build Phase](/images/dev-practices/ci-cd-pipeline/build-pipeline-wide.svg)

The build phase can be thought of as these steps.

### Commit

The build pipeline is triggered when code is pushed to the master branch - either by committing to it directly or by
merging in code. You can set up pipelines for separate branches/tags too, for example to support multiple releases.
A pipeline can also be set for PR branches, to ensure they compile/run when merged in or rebased on to master.

### Trigger

The build phase should be triggered whenever new code goes onto the branch. 
We are _not_ deploying in this phase, so it is fine for it to trigger automatically. 
To do this, add a hook from your CI tool to the version control host (e.g. BitBucket, GitHub) so that it can notify the
CI job when a branch is updated.
- this requires internet connectivity between the two services
- we need to download the code onto the CI job (usually a container) before we start
- this may require higher access controls (i.e. DevOps)

### Compile

This step is usually built into your build tool, but it helps to think of them as separate steps.
In the compile step, we ensure both the source code and test code compiles. As such, the pipeline step needs to be
able to download all the dependencies the code needs.
The build tool (e.g. mvn, gradle, npm) should take care of this for us.
Ideally want a clean slate every time, no prior source code on the machine, though we often cache the dependencies 
between runs (e.g. m2 folder) to reduce the pipeline execution time.

### Test

In this step, we want to ensure the unit / integration tests pass. 
Similar to the compile step, we want a cli tool to run this step. 
For web apps that utilize a browser there are usually headless options available. 

### Package

In this step, we want to create the deployable binary.
It should be an immutable 'fat' artifact. This means:
- it is not re-built during deployment
- it is not downloading additional code at runtime
- it is customized via configuration - either through environment vars or config server

By using this same artifact during deployment, instead of compiling and packaging it again at deployment, we can rule
out the possibility of dependencies being modified or the CI environment being polluted. This becomes important when
we are trying to debug a release or if we need to rollback to a previous artifact.

### Upload

After we package the artifact, we want to store it in a repository. If it was a library, we can now reuse it in our
project. If it was for deployment, it can be fetched in the deployment phase.

For public use, Maven Central and JCenter are used for many Apache Maven packages (for use in Maven, Gradle, etc).
NuGet is used for .NET packages, while npmjs is often used for web projects.
For private use, Nexus and Artifactory are well-known repository managers used by many companies. These typically
provide support for your framework's build system (e.g. maven, npm, nuget, etc) so that you can refer to these repos
instead of the public ones.  

### Notify

The last step is to notify the developer. For fast feedback loops, we recommend a dedicated screen/TV for CI/CD pipeline
status. You could also install plugins to notify Slack or send emails.

---

## Deployment Phase

![Deploy Phase](/images/dev-practices/ci-cd-pipeline/deploy-pipeline-wide.svg?width=50%)

The deployment phase can be thought of as these steps.

### Trigger

Unlike the build phase, we don't necessarily want to trigger the deployment phase automatically. For example:
- for lower environments (e.g. DIT), this could be updated asap and on-demand
    - developers want to test ASAP
- for story verification (e.g. SIT), this could be updated nightly and on-demand
    - less frequent deployment but more stable environment
    - external consumers of your app would use this version or higher
- for higher environments (e.g. Prod), this could be on-demand only
    - more strict and limited user access control
    - pipelines may be disabled when not in use


### Download

In this step, we download an existing artifact that we previously packaged and upload in the build phase.
We should not build again, as we described in the Package step of the build phase, and as this reduces execution time.
We want to use the same artifact that was tested by devs and verified by the product team.


### Deploy

In this step, we finally deploy our app to the platform. 
With Tanzu Application Service (TAS), we'd have a manifest present and use that to deploy.  

However, there's a few other things to consider here, such as:
- binding to the backing services
- zero downtime deployment


#### Binding to the backing services

Our services often bind to backing services - such mysql, redis, rabbitmq, etc. The apps assume these services exist
when they are deployed, otherwise they will fail at startup/runtime. 

It would be nice to automatically create any backing services if they do not exist as part of our pipeline.
That way we can deploy our apps to any space without having to manually reconfigure them each time.
This step should occur before the apps are deployed.  One caveat is that we'd need to wait for the service to be up and
running before continuing deployment. Afaik, there's no nice way to do this - sleep and check back every few ms to see
if the service is good to go. 


#### Zero Downtime Deployment

When many web services are being updated, often times there's a maintenance window where the app is inaccessible.
You may be able to apply a zero-downtime deployment strategy, such as Blue Green Deployment, to avoid this issue.

The specifics of Blue Green Deployment will be described in a separate page, but in essence, you'll temporarily have
two versions of your app running. Your new app will have a temporary url where you can verify the functionality.
Once you are ready to make the swap, you can migrate your users instantly - by having your old url point to the new app.
Alternatively, you could gradually your traffic. Once all the users are on the new app, you can shut down the old app.
An instant switch over is not too much work to build on a pipeline, but a gradual switch will likely be a manual
process.


### Notify

Your app is in prod - let everyone know it's time to party!

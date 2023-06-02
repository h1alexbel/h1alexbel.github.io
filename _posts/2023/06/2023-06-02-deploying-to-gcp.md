---
layout: post
date: 2023-06-02
place: Minsk, Belarus
title: "One Click Deployment to Google Cloud Compute Engine"
tags: devops
---
This is a short and simple guide to how to deploy a new version of your application to the Google Cloud Compute Engine,
just from GitHub issue. Fully automated. No scripts.

<!--more-->

Deploying an application to the [Google Cloud](https://cloud.google.com/) is very easy for me.
I just post a comment in the GitHub issue: `@rultor deploy, image is <image id>, instance is <instance> `.
[Rultor](http://www.rultor.com/), DevOps chatbot, deploys the application to [Compute Engine](https://cloud.google.com/compute) in just 5 minutes.
I don't have any local scripts or something like this.
I can even deploy a new version from browser on my phone.

How to do it? Follow the next steps:

## Prepare Google Cloud credentials

Get your JSON file with credentials from Google Cloud.
For security reasons, I am always transfer this file into a secret GitHub repo.
Let's name it `h1alexbel/app-secrets`.
There I create a folder called `assets` with my JSON Google Cloud Credentials file.

Also, don't forget about adding your repository with application into `.rultor.yml` as a "friend".
```yaml
friends:
  - h1alexbel/app
```

Your GCP Credentials file will be automatically fetched from `app-secrets` when a deployment process begins. 

## Create Rultor Configuration
Back to my repository with my application.
In the root directory I commit the next file:

```yaml
architect:
  - h1alexbel # your GitHub nickname
assets:
  creds.json: h1alexbel/app-secrets#assets/creds.json
docker:
  image: l3r8y/rultor-image:1.0.3
install: |-
  sudo apt install python3.8
  curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-429.0.0-linux-x86_64.tar.gz
  tar -xf google-cloud-cli-429.0.0-linux-x86_64.tar.gz
# it will disable interactive prompts for chatbot
  CLOUDSDK_CORE_DISABLE_PROMPTS=1 ./google-cloud-sdk/install.sh 
deploy:
  script:
    - "./google-cloud-sdk/bin/gcloud auth login --cred-file=../creds.json"
    - "./google-cloud-sdk/bin/gcloud config set project <PROJECT ID>"
    - "./google-cloud-sdk/bin/gcloud config configurations activate default"
    - "./google-cloud-sdk/bin/gcloud compute instances update-container $instance --container-image=$image"
```

## Run It!
Now we can run it. Create a new issue on GitHub and post a comment:

```text
@rultor deploy, image is <docker image id>, instance is <Compute Engine INSTANCE ID>
```

You will get a response in a few seconds. Rultor will do the rest.
Enjoy deploying from just a browser on your phone like me :)

Also, read in details about Rultor, the interactive DevOps GitHub-based chatbot.

[Documentation](https://doc.rultor.com/).
[_Rultor.com, a Merging Bot_](https://www.yegor256.com/2014/07/24/rultor-automated-merging.html).

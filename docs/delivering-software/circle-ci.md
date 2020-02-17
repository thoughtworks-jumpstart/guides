# Circle Ci

## Introduction

[CircleCI](https://circleci.com/) - Our mission is to empower technology-driven organisations to do their best work.
We want to make engineering teams more productive through intelligent automation.

Manual deployment is a tedious and error-prone job. Running through all the steps of checking lint, testing, security scans, build, setting up the environment, running e2e test, provisioning network access or data, etc.

### Overhead of doing manual deployment

![manual deployment](_media/manualOverhead.png)

Morden days CD tools will take care of all these manual work for us once we create a pipeline.

A pipeline is a workflow that runs jobs we can configure. Example if we have three jobs, run lint checks, run unit test, deploy to Heroku. We can set the pipeline to run lint checks, if pass, we then run the unit test, and again if pass, we then deploy to Heroku.

![manual deployment](_media/circleci-pipeline.png)

Circle CI is simple to use, provide excellent features and have a free tier.

### Alternative tools in the market

1. [Jenkins](https://jenkins.io/)
2. [gocd](https://www.gocd.org/)
3. [Bamboo](https://www.atlassian.com/software/bamboo)
4. [Travis](https://travis-ci.org/)
5. [TeamCity](https://www.jetbrains.com/teamcity/)

## Circle CI setup

Signup CircleCI with your GitHub account.
https://circleci.com/signup/.

Signing up with GitHub also gives circle ci rights to access your repo and allow you to configure a new project.

Click on the Add project tab on the left and add a project.

![manual deployment](_media/circleci-addProject.png)

Look for the GitHub repo and select "Set Up Project".

You will see a sample `config.yml` generated for you.

```yaml
version: 2
jobs:
  build:
    docker:
      - image: circleci/node:7.10
    working_directory: ~/repo
    steps:
      - checkout
      - restore_cache:
          keys:
            - v1-dependencies-{{ checksum "package.json" }}
            - v1-dependencies-
      - run: yarn install
      - save_cache:
          paths:
            - node_modules
          key: v1-dependencies-{{ checksum "package.json" }}
      - run: yarn test
```

### YAML

Files with this `.yml` extensions are YAML files.

YAML stands for "YAML Ain't Markup Language", like JSON and XML, is another way of presenting your data and a serializable language.

Position of the indentation is crucial in YAML.
The data representation below, in JSON and YAML, are equivalent.

```json
{
  "person": {
    "firstName": "John",
    "lastName": "Smith",
    "age": 17,
    "hobbies": ["fencing", "basket ball", "reading", "running"],
    "contacts": [
      {
        "name": "Alice",
        "mobile": 6353535
      },
      {
        "name": "Bob",
        "mobile": 67773777
      }
    ]
  }
}
```

```yaml
person:
  firstName: John
  lastName: Smith
  age: 17
  hobbies:
    - fencing
    - basket ball
    - reading
    - running
  contacts:
    - name: Alice
      mobile: 6353535
    - name: Bob
      mobile: 67773777
```

### Docker

CircleCi uses docker.

Docker is a set of platform as a service products that use OS-level virtualisation to deliver software in packages called containers. - Wikipedia

In simple, docker works like a VM and gives us an environment that we can run our application. In Docker term, this environment is called a "Container".

Docker doesn't know about what our application does and will not recognise the dependencies we need. For docker to create a container that has the dependencies, we will need a docker image. A Docker image is the prewritten instructions or blueprints to create a Container. Ideally, we want a Container that already can run Node, and has access to git. CircleCI have already written the images for us. All we need to do is tell CircleCi which Docker Image to use.

```yaml
build:
  docker:
    - image: circleci/node:12.16
```

We can specify a working directory where all our files in the Docker container will be. It is generally a good idea to put all your files in a folder within docker. `working_directory: ~/repo`

### Workflow

By default, CircleCi runs all your job sequentially. We can more tell CircleCi specifically how we want to run the jobs using workflow.

The below workflow tell CircleCi to run two jobs, "test" and "deploy". "deploy" will only start to run after "test" runs successfully.

```yaml
workflows:
  version: 2
  run:
    jobs:
      - test
      - deploy:
          requires:
            - test
```

### Deploying to Heroku

Sidenote: Please make sure the application is already working, and you can push the app to Heroku manually. On local: `git push heroku master` and verify the application works.

Heroku store the code we deployment code in their git repository. Using the `git push` and by specifying the API_KEY and APP_NAME, we can trigger a git push to Heroku from anywhere.

```yaml
deploy:
  working_directory: ~/app
  docker:
    - image: buildpack-deps:trusty
  steps:
    - checkout
    - run:
        name: Deploy Master to Heroku
        command: git push https://heroku:$HEROKU_API_KEY@git.heroku.com/$HEROKU_APP_NAME.git master
```

To find the API KEY, Install Heroku CLI and login and create a key.

```bash
heroku login
heroku authorisations:create
```

Add the key/token into the circle ci environment variable, so it has access to it.

![env var](_media/circleci-env.png)

On your next push, you will see the jobs in action in the CircleCi dashboard.

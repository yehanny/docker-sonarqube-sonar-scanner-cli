# Docker Compose - Sonarqube / Sonar Scanner CLI / GITHUB

Sonarqube is an open source platform for continuous inspection of code quality.

## Table of contents

1. [Introduction](#introduction)
2. [Installation](#installation)
3. [Support](#support)
4. [Optional](#optional)
5. [License](#license)

## Introduction

Sonarqube is an open source platform for continuous inspection of code quality. The platform can be used to perform automatic reviews with static analysis of code to detect bugs, code smells and security vulnerabilities on 20+ programming languages including Java, C#, JavaScript, TypeScript, C/C++, COBOL and more. SonarQube is the only product on the market that supports a leak approach as a practice to code quality.

## Installation

You can copy all the files inside this repository in your project, also you'll need https://www.docker.com/ and https://docs.docker.com/compose/ to run the commands inside your local environment

### SonarQube + Postgres DB

Copy the .env.example to .env inside the repository

Run first the sonarqube + db services. SonarQube server will take some minutes to deploy and we can't run the Sonar Scanner until this first task is done, we also need to setup our project and if we're using for example GitHub we'll need to:

### GitHub App Setup

- Create a GitHub App from Settings > Developer Settings > GitHub App to get our API Credentials, Secret and Private Key
- Install that new App for a single or multiple repositories to connect to it.
- Setup your Repository Permissions:

* Checks: Read and Write
* Commit statuses: Read
* Metadata: Read
* Pull requests: Read and Write

Save and next copy your credentials because we're gonna need it for the next step

- App ID:
- Client ID:
- Client secrets:
- Private key:

Now it's time to deploy our SonarQube instance, run:

```bash
$ docker compose up -d sonarqube db
```

Then double check both services are mounted and runnning and you should see something like this

```bash
$ docker compose ps
```

NAME                COMMAND                  SERVICE             STATUS              PORTS
postgresql          "docker-entrypoint.s…"   db                  running             5432/tcp     
sonarqube           "/opt/sonarqube/bin/…"   sonarqube           running             0.0.0.0:9000->9000/tcp, :::9000->9000/tcp

Go to your SonarQube server http://localhost:9000 and login with the default credentials. User: admin Pass: admin and then change them to your custom password, save and go to > Projects and select GitHub and use your App credentials that we created before to fill the form

### Running the Sonar Scanner

* Setup your .env Variables with the one that SonarQube just created for us

```bash
SONAR_LOGIN=
SONAR_PROJECT_KEY=
SONAR_SOURCES=
```

Now it's time to run our first test using this command

```bash
$ docker compose up sonar-scanner-cli
```

Wait for the test to finnish and go to http://localhost:9000 to see your results! Isn't that awesome? Happy Testing 

## Optional

### vm.max_map_count Issue

You may have vm.max_map_count size issue and for that I've created a bash file to solve it add execute permissions to the file sonarqube-init.sh

* On Linux

```bash
chmod +x sonarqube-init.sh
```

* On MacOS and Windows

```bash
chmod 755 sonarqube-init.sh
```

Then run it: ./sonarqube-init.sh


### SonarQube Properties file

You can setup the properties of your project using the sonar-project.properties file

```bash
# must be unique in a given SonarQube instance
sonar.projectKey=sample

# --- optional properties ---

# defaults to project key
sonar.projectName=My project
# defaults to 'not provided'
sonar.projectVersion=1.0.0
 
# Path is relative to the sonar-project.properties file. Defaults to .
sonar.sources=.
 
# Encoding of the source code. Default is default system encoding
sonar.sourceEncoding=UTF-8
```

## Support

Don't hesitate to comment if you have any issue and I'll help you for sure to solve it, I'm 24/7 in my email

If you want to support this effort and time on doing this I'll be so grateful with you (And with God)
You can https://www.buymeacoffee.com/yehanny

## License

[MIT](LICENSE)

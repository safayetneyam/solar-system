# Solar System NodeJS Application

A simple HTML+MongoDB+NodeJS project to display Solar System and it's planets.

---
## Requirements

For development, you will only need Node.js and NPM installed in your environement.

### Node
- #### Node installation on Windows

  Just go on [official Node.js website](https://nodejs.org/) and download the installer.
Also, be sure to have `git` available in your PATH, `npm` might need it (You can find git [here](https://git-scm.com/)).

- #### Node installation on Ubuntu

  You can install nodejs and npm easily with apt install, just run the following commands.

      $ sudo apt install nodejs
      $ sudo apt install npm

- #### Other Operating Systems
  You can find more information about the installation on the [official Node.js website](https://nodejs.org/) and the [official NPM website](https://npmjs.org/).

If the installation was successful, you should be able to run the following command.

    $ node --version
    v8.11.3

    $ npm --version
    6.1.0

---
## Install Dependencies from `package.json`
    $ npm install

## Run Unit Testing
    $ npm test

## Run Code Coverage
    $ npm run coverage

## Run Application
    $ npm start

## Access Application on Browser
    http://localhost:3000/

---

# CI/CD with GitHub Actions

This project uses **GitHub Actions** for Continuous Integration and Continuous Deployment (CI/CD), enabling automated testing, building, and deployment across multiple environments.

## Environments

Two environments are configured in GitHub:

- **development**
- **production**

Each environment contains:
- Environment-specific **secrets**
- Environment-specific **variables**
- Optional protection rules (e.g., wait time, required reviewers)



## Environment Configuration

### Production Environment

- **Secret**
  - `KUBECONFIG`  
    Stores the Kubernetes configuration file for accessing the production cluster.

- **Variables**
  - `NAMESPACE = production`
  - `REPLICAS = 5` *(example value)*


### Development Environment

- **Secret**
  - `KUBECONFIG`  
    Stores the Kubernetes configuration file for accessing the development cluster.

- **Variables**
  - `NAMESPACE = development`
  - `REPLICAS = 2` *(example value)*

---

## Repository Secrets

The following secrets are configured at the repository level and used in workflows.

### DigitalOcean Spaces (Log Storage)

Used as an alternative to AWS S3 for storing logs and artifacts:

- `DO_SPACES_ACCESS_KEY`
- `DO_SPACES_SECRET_KEY`
- `DO_SPACES_BUCKET`
- `DO_SPACES_REGION`


### Pull Request Automation

- `GIPHY_API_KEY`  
  Used to generate automated comments on pull requests via custom GitHub Actions (Docker and JS Action).

---

### Workflow Credentials

- `MONGO_PASSWORD`  
  Used for MongoDB authentication.

- `DOCKER_PASSWORD`  
  Used for Docker registry authentication.

---

## Repository Variables

The following variables are used across workflows:

- `DOCKER_USERNAME`
- `MONGO_URI`  
  MongoDB connection string to connect to the database.
- `MONGO_USERNAME`
- `REPLICAS`

---
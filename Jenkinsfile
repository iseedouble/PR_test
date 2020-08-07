pipeline {
  environment {
        HOME = "."
        nexusDockerRegistryUrl = "docker-production.bnc.ca"
    }
    parameters {
        choice(
            name: "deploymentStage",
            choices: "development\nstaging", 
            description: "The deployment stage / sub-stage."
        )
        string(
            name: "version",
            description: "The component version.",
            defaultValue: "latest"
        )
    }
    options {
        disableConcurrentBuilds()
    }
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            echo 'Building the core application'
          }
        }

        stage('Test') {
          steps {
            echo 'Testing the application'
          }
        }

      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying the application in server'
      }
    }

  }
}
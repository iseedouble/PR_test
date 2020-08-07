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
    stage("Initialize"){
        steps {
            script {
                /* Uses the Jenkins folders tree to determine the various parameters. */
                (applicationId, environment, pipelineName) = env.JOB_NAME.split("/")[0..2]
                componentName = pipelineName.replace('-deployment', '') // Remove 'deployment'. 
                
                /* Loads the Tool stack config file created by the enroller to get tool stack IDs. */
                configFileProvider([configFile(fileId: "tool-stacks", targetLocation: "tool-stacks.json")]) {
                    def toolStacksConfig = readJSON file: "tool-stacks.json"
                    toolStackId = toolStacksConfig.active
                    artifactManagementCredentialStoreEntry = "${toolStackId}.artifact-management"
                }
                /* Loads the secret manager (vault) configuration. */
                configFileProvider([configFile(fileId: "${toolStackId}.secret-management", targetLocation: "secret-management.json")]) {
                    secretManagementConfig = readJSON file: "secret-management.json"
                }
                /* Load the webhook url for teams notification*/
            //     configFileProvider([configFile(fileId: "ci-cd-status-teamhook", targetLocation: "ci-cd-status-teamhook.json")]) {
            //        teamsHookUrlConfig = readJSON file: "ci-cd-status-teamhook.json"
            //        hookUrl = teamsHookUrlConfig.hookUrl
            //        echo "teams hook url is : ${hookUrl}"
            //    }
            }
        }
      }
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            echo 'Building the core application 1'
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
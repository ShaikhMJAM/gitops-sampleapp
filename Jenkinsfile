pipeline {
    agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "sample-reactapp"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/ShaikhMJAM/gitops-sampleapp'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat sample-appdeployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' sample-appdeployment.yaml
                   cat sample-appdeployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "ShaikhMJAM"
                   git config --global user.email "jaid.shaikh@acuteinformatics.in"
                   git add sample-appdeployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/ShaikhMJAM/gitops-sampleapp.git main"
                }
            }
        }
      
    }
}

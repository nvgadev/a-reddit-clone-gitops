pipeline {
    agent any
    parameters {
        string(name: 'IMAGE_TAG', defaultValue: '', description: '')
    }
    environment {
          APP_NAME = "reddit-clone-pipeline"
    }
    stages {
         stage("Cleanup Workspace") {
             steps {
                cleanWs()
             }
         }
         stage("Checkout from SCM") {
             steps {
                     // git branch: 'main', credentialsId: 'github_UP', url: 'https://github.com/nvgadev/a-reddit-clone-gitops'
                     git branch: 'main', url: 'https://github.com/nvgadev/a-reddit-clone.git'
             }
         }
         stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
         }
         stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "nvgadev"
                    git config --global user.email "gopi.dev999@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github_UP', gitToolName: 'Default')]) {
                    sh "git push https://github.com/nvgadev/a-reddit-clone.git main"
                }
            }
         }
    }
}

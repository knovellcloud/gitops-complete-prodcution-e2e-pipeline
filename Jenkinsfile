pipeline {
    agent {
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "complete-production-e2e-pipeline"
    }
    stages {
        stage("Clean Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main',  url: 'https://github.com/knovellcloud/gitops-complete-prodcution-e2e-pipeline'
            }
        }

        // IMAGE_TAG is passed by another job
        stage("Update the Development Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment to Git") {
            steps {
                sh """
                    git config --global user.name "knovellcloud"
                    git config --global user.email "isjinjia.zhang@gmail.com"
                    git add deployment.yaml
                    git commit -m "Update the deployment manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github-pat',
                 gitToolName: 'Default')]) {
                    sh "git push https://github.com/knovellcloud/gitops-complete-prodcution-e2e-pipeline main"
                }
            }
        }
    }
}
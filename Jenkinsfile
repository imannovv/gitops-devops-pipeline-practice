pipeline{

    agent {
        label "jenkins-agent"
    }

    environment {
        APP_NAME = "devops-pipeline-practice"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/imannovv/gitops-devops-pipeline-practice'
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

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                    git config --global user.name "Mahammad Imanov"
                    git config --global user.email "imannovv@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github-pat', gitToolName: 'Default')]) {
                    sh "git push https://github.com/imannovv/gitops-devops-pipeline-practice main"
                }
            }
        }
    }
}
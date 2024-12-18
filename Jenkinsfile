pipeline {
    agent {
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "minhnhan548"
    }

    stages {
        stage ("Cleanup workspace") {
            steps {
                cleanWs()
            }
        }

        stage ("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/nhanalvth/gitops-complete-prodcution-e2e-pipeline-main'
            }
        }

        stage ("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }

        }

        stage ("Push the change deployment file to Git") {
            steps {
                sh """
                    git config --global user.name "nhanalvth"
                    git config --global user.email "nguyenminhnhanalvth@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Menifest"
                """
                //withCredentials([gitUsernamePassword{credentialsId: 'github', gitToolName: 'Default'}]) {
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]){
                    sh "git push https://github.com/nhanalvth/gitops-complete-prodcution-e2e-pipeline-main main"
                }
            }
        }
    }
}
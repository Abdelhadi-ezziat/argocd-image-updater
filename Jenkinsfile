pipeline {
    agent {
        label "jenkins-agent"
    }

    environment {
        APP_NAME = "complete-devops-pipeline"
        GITHUB_TOKEN = credentials('github')
    }

    stages {
        stage("Cleanup workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github-token', url: 'https://github.com/Abdelhadi-ezziat/gitops-argocd-image-updater'
            }
        }

        stage("Update image tag") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }

        // stage("Push changes to repo") {
        //     steps {
        //         script {
        //             def githubToken = credentials("github-token") // Use the correct environment variable for your GitHub token
        //             def gitUrl = "https://abdelhadi-ezziat:${githubToken}@github.com/Abdelhadi-ezziat/gitops-argocd-image-updater"
                    
        //             sh """
        //                 git config --global user.name "abdelhadi-ezziat"
        //                 git config --global user.email "abdelhadi.ezziat@gmail.com"
        //                 git add deployment.yaml
        //                 git commit -m "Updated deployment manifest"
        //                 git push ${gitUrl} main
        //             """
        //         }
        //     }
        // }
        stage("Push changes to repo") {
            steps {
                sh """
                    git config --global user.name "Abdelhadi-ezziat"
                    git config --global user.email "ezziat.abdelhadi@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated deployment manifest"
                    git remote set-url origin https://$GITHUB_TOKEN@github.com/Abdelhadi-ezziat/argocd-image-updater.git
                    git push origin main
                """
                // git -c http.sslVerify=false push https://Abdelhadi-ezziat:<token>@github.com/Abdelhadi-ezziat/gitops-argocd-image-updater.git main
                // git push https://abdelhadi-ezziat:${github-token}@github.com/Abdelhadi-ezziat/gitops-argocd-image-updater main
                // withCredentials{[gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]} {
                //     sh "git push https://github.com/Abdelhadi-ezziat/gitops-argocd-image-updater.git main"
                // }

            }
        }
    }
}
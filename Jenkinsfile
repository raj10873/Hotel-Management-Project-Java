pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/raj10873/Hotel-Management-Project-Java.git'
    }

    stages {
        stage('Merge feature-code into development-code') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'raj-git', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    script {
                        gitMerge('feature-code', 'development-code', env.GIT_USERNAME, env.GIT_PASSWORD)
                    }
                }
            }
        }

        stage('Merge development-code into release-code') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'raj-git', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    script {
                        gitMerge('development-code', 'release-code', env.GIT_USERNAME, env.GIT_PASSWORD)
                    }
                }
            }
        }

        stage('Merge release-code into master') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'raj-git', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    script {
                        gitMerge('release-code', 'master', env.GIT_USERNAME, env.GIT_PASSWORD)
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'All merges completed successfully!'
        }
        failure {
            echo 'Merge failed! Please check conflicts or authentication issues.'
        }
    }
}

def gitMerge(String sourceBranch, String targetBranch, String username, String password) {
    sh """
        git config --global user.email "jenkins@example.com"
        git config --global user.name "Jenkins"

        # Clean workspace and clone using credentials
        rm -rf repo
        git clone https://${username}:${password}@github.com/raj10873/Hotel-Management-Project-Java.git repo
        cd repo

        # Checkout target branch
        git checkout ${targetBranch}
        git pull origin ${targetBranch}

        # Merge source branch into target branch
        git merge --no-ff origin/${sourceBranch} -m "Merge ${sourceBranch} into ${targetBranch}"

        # Push changes back using credentials
        git push https://${username}:${password}@github.com/raj10873/Hotel-Management-Project-Java.git ${targetBranch}
    """
}

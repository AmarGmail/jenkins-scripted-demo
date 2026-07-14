pipeline {
    agent any

    stages {
        stage('Checkout code'){
            steps {
                checkout scm
            }
        }
        stage('print environment'){
            steps {
                sh 'printenv | sort'
            }
        }

        stage('git info') {
            steps {
                sh '''
                echo
                echo "======GIT========="
                git rev-parse HEAD
                git branch -vv
                git remote -v
                '''
            }
        }
    }
}
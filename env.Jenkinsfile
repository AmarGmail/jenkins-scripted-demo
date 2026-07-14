pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                script {
                    echo "===== Checking out SCM ====="

                    def scmVars = checkout scm

                    echo "===== SCM Variables ====="
                    scmVars.each { key, value ->
                        echo "${key} = ${value}"
                    }
                }
            }
        }

        stage('Jenkins Variables') {
            steps {
                script {
                    echo "===== Selected Jenkins Variables ====="

                    [
                        'JOB_NAME',
                        'JOB_BASE_NAME',
                        'BUILD_NUMBER',
                        'BUILD_ID',
                        'BUILD_URL',
                        'WORKSPACE',
                        'NODE_NAME',
                        'BRANCH_NAME',
                        'CHANGE_ID',
                        'CHANGE_BRANCH',
                        'CHANGE_TARGET',
                        'GIT_COMMIT',
                        'GIT_BRANCH',
                        'GIT_URL',
                        'JENKINS_URL'
                    ].each { key ->
                        echo "${key} = ${env[key]}"
                    }
                }
            }
        }

        stage('Linux Environment') {
            steps {
                sh '''
                    echo "===== printenv ====="
                    printenv | sort
                '''
            }
        }

        stage('Git Information') {
            steps {
                sh '''
                    echo
                    echo "===== Git Version ====="
                    git --version

                    echo
                    echo "===== Current Commit ====="
                    git rev-parse HEAD

                    echo
                    echo "===== Branch ====="
                    git branch -vv

                    echo
                    echo "===== Status ====="
                    git status

                    echo
                    echo "===== Remotes ====="
                    git remote -v

                    echo
                    echo "===== Last Commit ====="
                    git log --oneline -1
                '''
            }
        }

        stage('Workspace') {
            steps {
                sh '''
                    echo
                    echo "===== Workspace ====="
                    pwd

                    echo
                    echo "===== Files ====="
                    ls -la
                '''
            }
        }

        stage('Build Information') {
            steps {
                script {

                    echo "===== Build Information ====="

                    echo "JOB_NAME            = ${env.JOB_NAME}"
                    echo "JOB_BASE_NAME       = ${env.JOB_BASE_NAME}"
                    echo "BUILD_NUMBER        = ${env.BUILD_NUMBER}"
                    echo "BUILD_ID            = ${env.BUILD_ID}"
                    echo "BUILD_URL           = ${env.BUILD_URL}"
                    echo "WORKSPACE           = ${env.WORKSPACE}"
                    echo "NODE_NAME           = ${env.NODE_NAME}"
                    echo "EXECUTOR_NUMBER     = ${env.EXECUTOR_NUMBER}"
                    echo "BRANCH_NAME         = ${env.BRANCH_NAME}"
                    echo "CHANGE_ID           = ${env.CHANGE_ID}"
                    echo "CHANGE_BRANCH       = ${env.CHANGE_BRANCH}"
                    echo "CHANGE_TARGET       = ${env.CHANGE_TARGET}"
                    echo "GIT_COMMIT          = ${env.GIT_COMMIT}"
                    echo "GIT_BRANCH          = ${env.GIT_BRANCH}"
                    echo "GIT_URL             = ${env.GIT_URL}"
                    echo "JENKINS_URL         = ${env.JENKINS_URL}"
                }
            }
        }
    }

    post {
        always {
            echo "===== Debug Pipeline Completed ====="
        }
    }
}
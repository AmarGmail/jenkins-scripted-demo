timestamps{
    node {
        try{
            stage('Checkout'){
                echo "Checkout the Git code...."
                checkout scm
                echo "Git commit id: ${env.GIT_COMMIT}"
                githubNotify(
                    credentialsId: 'github-pat',
                    status: 'PENDING',
                    description: "Build #${env.BUILD_NUMBER} in progress"
                )
            }
            
            stage('Create Python Virtual Environment'){
                echo "Creating python venv..."
                if (isUnix()) {
                    sh 'python3 -m venv venv'
                } else {
                    bat 'python -m venv venv'
                }
            }

            stage('Install Dependencies'){
                echo "Install Python Dependencies..."
                if (isUnix()) {
                    sh './venv/bin/python -m pip install -r requirements.txt'
                } else {
                    bat '.\\venv\\Scripts\\python.exe -m pip install -r requirements.txt'
                }
            }

            stage('Run Application'){
                echo "Running Python Application..."
                if (isUnix()) {
                    sh './venv/bin/python app.py'
                } else {
                    bat '.\\venv\\Scripts\\python.exe app.py'
                }
            }

            stage('Run Unit Test Cases'){
                echo "Running Automated Test Cases..."
                try {
                    if (isUnix()) {
                        sh './venv/bin/pytest -v --junitxml=test-results.xml'
                    } else {
                        bat '.\\venv\\Scripts\\pytest -v --junitxml=test-results.xml'
                    }
                } finally {
                    junit allowEmptyResults: true,
                         testResults: 'test-results.xml'
                }
            }
            
            stage('GitHub Notify'){
                echo "The pipeline completed successfully."
                githubNotify(
                    credentialsId: 'github-pat',
                    status: 'SUCCESS',
                    description: "Build #${env.BUILD_NUMBER} passed"
                )
            }

        } catch (Exception e) {
            githubNotify(
                credentialsId: 'github-pat',
                status: 'FAILURE',
                description: "Build #${env.BUILD_NUMBER} failed"
            )
            throw e
        } finally {
            echo "Cleaning the workspace..."
            deleteDir()
        }
    } 
}
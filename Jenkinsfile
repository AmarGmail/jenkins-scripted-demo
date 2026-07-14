timestamps{
    node {
        try{
            stage('Checkout'){
                echo "Checkout the Git code...."
                checkout scm

                githubNotify(
                    credentialsId: 'github-pat',
                    status: 'PENDING',
                    description: 'build started'
                )
            }
            stage('Show Files'){
                echo "Displaying the files..."
                if (isUnix()){
                    sh 'ls -l'
                } else {
                    bat 'dir'
                }

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
                    bat './venv/bin/python app.py'
                }
            }
            stage('Run Unit Test Cases'){
                echp "Running Automated Test Cases..."
                if (isUnix()) {
                    sh './venv/bin/pytest -v --junitxml=test-results.xml'
                } else {
                    bat '.\\venv\\Scripts\\pytest -v --junitxml=test-results.xml'
                }

            }
            stage('Publish The Test Results'){
                echo "JUNIT Test Results..."
                junit 'test-results.xml'
            }
            stage('success'){
                echo "The pipeline completed successfully."

            }
        } catch (Exception e) {
            currentBuild.result = 'FAILURE'
            throw e
        } finally {
            echo "Cleaning the workspace..."
            deleteDir()
        }
    } 
}
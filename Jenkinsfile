node {
    try {
        stage('Checkout') {
            echo "Checking out the code .."
            checkout scm
        }
        stage('show files') {
            echo "Displaying the files..."
            if(isUnix()) {
                sh 'ls -l'
            } else {
                bat 'dir'
            }
        }
        stage('Install Dependencies') {
            echo "Install python dependencies...."
            if (isUnix()) {
                sh 'python3 -m pip install -r requirements.txt'
            } else {
                bat 'python -m pip install -r requirements.txt'
            }
        }
        stage('Run Application') {
            echo "Running the Python Application"
            if (isUnix()) {
                sh 'python3 app.py'
            } else {
                bat 'python app.py'
            }
        }
        stage('Run Test Cases') {
            echo "Running the Automated Test..."
            if (isUnix()) {
                sh 'pytest -v'
            } else {
                bat 'pytest -v'
            }
        }
        stage('Build Result') {
            echo "Application Build and Test completed successfully"
        }

    } catch (Exception e) {
        echo "An error occurred: ${e.getMessage()}"
        currentBuild.result = 'FAILURE'
    }
    finally {
        echo "Cleaning the workspace...."
        //cleanWs()
        deleteDir()
    }
}
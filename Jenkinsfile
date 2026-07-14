node {
    try {
        stage('Checkout') {
            echo "Checking out the code .."
            checkout scm
        }
        stage('Show Files') {
            echo "Displaying the files..."
            if(isUnix()) {
                sh 'ls -l'
            } else {
                bat 'dir'
            }
        }   
        stage('Create python virtual environment') {
            echo "Createing python venv..."
            if (isUnix()) {
                sh '''
                python3 -m venv venv
                ./venv/bin/python -m pip install --upgrade pip
                '''
            } else {
                bat '''
                python -m venv venv
                .\\venv\\Scripts\\python.exe -m pip install --upgrade pip
                '''
            }
        }
        stage('Install Dependencies') {
            echo "Install python dependencies...."
            if (isUnix()) {
                sh './venv/bin/python -m pip install -r requirements.txt'
            } else {
                bat '.\\venv\\Scripts\\python.exe -m pip install -r requirements.txt'
            }
        }
        stage('Run Application') {
            echo "Running the Python Application"
            if (isUnix()) {
                sh './venv/bin/python app.py'
            } else {
                bat '.\\venv\\Scripts\\python.exe app.py'
            }
        }
        stage('Run Test Cases') {
            echo "Running the Automated Test..."
            if (isUnix()) {
                sh './venv/bin/pytest -v'
            } else {
                bat '.\\venv\\Scripts\\pytest -v'
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
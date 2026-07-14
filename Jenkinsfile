
timestamps {
node {
    try {
        stage('Checkout') {
            echo "Checking out the code .."
            checkout scm

            githubNotify(
                credentialsId: 'github-pat',
                account: 'AmarGmail',
                repo: 'jenkins-scripted-demo',
                status: 'PENDING',
                description: "Build #${env.BUILD_NUMBER} started",
                context: 'ci/jenkins'
            )
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
                //sh '''
                //python3 -m venv venv
                //./venv/bin/python -m pip install --upgrade pip
                //'''
                sh 'python3 -m venv venv'
            } else {
                //bat '''
                //python -m venv venv
                //.\\venv\\Scripts\\python.exe -m pip install --upgrade pip
                //'''
                bat 'python -m venv venv'
            }
        }
        stage('Install Dependencies') {
            echo "Install Python Dependencies...."
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
                sh './venv/bin/pytest -v --junitxml=test-results.xml'
            } else {
                bat '.\\venv\\Scripts\\pytest -v --junitxml=test-results.xml'
            }
        }
        stage('Publish the Test Results'){
            junit 'test-results.xml'
        }
        stage('Build Result') {
            echo "Application Build and Test completed successfully"

            githubNotify(
                credentialsId: 'github-pat',
                account: 'AmarGmail',
                repo: 'jenkins-scripted-demo',
                status: 'SUCCESS',
                description: "Build #${env.BUILD_NUMBER} passed",
                context: 'ci/jenkins'
            )
        }

    } catch (Exception e) {
        echo "An error occurred: ${e.getMessage()}"
        currentBuild.result = 'FAILURE'
        githubNotify(
            credentialsId: 'github-pat',
            account: 'AmarGmail',
            repo: 'jenkins-scripted-demo',
            status: 'FAILURE',
            description: "Build #${env.BUILD_NUMBER} failed",
            context: 'ci/jenkins'
        )

throw e
    }
    finally {
        echo "Cleaning the workspace...."
        //cleanWs()
        deleteDir()
    }
}
}
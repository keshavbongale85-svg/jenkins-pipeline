
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat 'if not exist build mkdir build'
                bat 'javac -d build src\\Main.java'
                echo 'Build completed successfully!'
            }
        }

        stage('Test') {
            steps {
                bat 'java -cp build Main | findstr /C:"Hello from Jenkins Pipeline!"'
                echo 'Test passed successfully!'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'build/*.class',
                                 fingerprint: true
            }
        }
    }
}
pipeline {
    agent any
    stages {
        stage('---clean---') {
            steps {
                echo "mvn clean"
            }
        }
        stage('--test--') {
            steps {
                echo "mvn test"
            }
        }
        stage('--build--') {
            steps {
                sh "mvn clean package"
                sh "docker build . -t tomcatwebapp:${env.BUILD_ID}"

            }
        }
    }
}

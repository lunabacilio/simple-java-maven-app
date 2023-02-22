pipeline {
    agent {
        docker {
            image 'maven:3.9.0-eclipse-temurin-11' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Scan') {
            environment {
                scannerHome = tool 'sonar-scanner'
            }
            steps{
                withSonarQubeEnv('sonarqube') { 
                    sh 'mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=str \
                        -Dsonar.host.url=http://sonarqube:9000 \
                        -Dsonar.login=squ_9c4f55428ecec85b21a69072cc570dbebb35a0db'
                }
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }   
}
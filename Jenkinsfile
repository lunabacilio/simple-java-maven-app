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
            //environment {
            //    scannerHome = tool 'sonar-scanner'
            //}
            steps{
                withSonarQubeEnv('sonarqube') { 
                    sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=simple-java-maven-app -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=sqp_42db6d5ed10fe7c23f6c7dcd8e19a2fe585e26bc'
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
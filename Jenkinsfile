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
            //environment{
                //    FILE = credentials('sonarqube-tkn')
                //}
            steps{
                //withCredentials([sonarqube-tkn(credentialsId: 'sonarqube-tkn', variable: 'FILE')]){
                //    echo '\${FILE}'
                //}
                //
                withSonarQubeEnv(credentialsId: 'sonar',installationName:'sonarqube') {
                    sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=sample-project -Dsonar.host.url=http://13.52.137.156:9000'
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
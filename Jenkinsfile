pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
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
        stage('Deliver'){
            steps{
                sh './jenkins/scripts/deliver.sh'
            }
        }
        stage('Check Jar location'){
            steps{
                sh "ls -al ./target/*.jar"
            }
        }
        stage('Build Docker Image'){
            steps{
                sh 'docker build -t bala-app:1.0.0 .'
            }
        }
        stage('Run Container'){
            steps{
                sh 'docker run -p 8181:8080 -d -name myapp bala-app:1.0.0'
            }
        }
    }
}

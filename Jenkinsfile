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
        stage('Copy jar file'){
            steps{
                sh 'cp ./target/*.jar /var/jenkins_home/workspace/docker_image_build/'
            }
        }
    }
}

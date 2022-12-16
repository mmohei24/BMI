pipeline{
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DockerHub')
    }
    stages{
        stage("Checkout"){
            steps{
                git 'https://github.com/mohei20/BMI-Calculator-App-Pipeline.git'
            }
        }
        stage("Build The App"){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('JUnit Tests'){
                steps {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
        }
        stage("Build Docker Image"){
            steps{
                sh 'docker build -t mmohei/bmi-calculator-image:v1 .'
            }
        }
        stage('Login To Dockerhub'){
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin '
            }
        }
        stage("Push Docker Image"){
            steps{
                sh 'docker push mmohei/bmi-calculator-image:v1'
            }
        }
    }
    post{
        always{
            sh 'docker logout'
        }
    }
}
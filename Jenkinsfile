pipeline{
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credintial')
    }
    stages{
        stage("Checkout"){
            steps{
                git 'https://github.com/mmohei24/BMI.git'
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
                sh 'docker build -t mmohei24/study:$BUILD_NUMBER .'
            }
        }
        stage('Login To Dockerhub'){
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin '
            }
        }
        stage("Push Docker Image"){
            steps{
                sh 'docker push mmohei24/study:$BUILD_NUMBER'
            }
        }
    }
    post{
        always{
            sh 'docker logout'
        }
    }
}

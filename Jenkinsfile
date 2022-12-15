pipeline{
    agent any

    stages{
        stage("Checkout"){
            steps{
                git 'https://github.com/mohei20/BMI-Calculator-App.git'
            }
        }
        stage("Build"){
            steps{
                pwsh 'mvn clean package'
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
                pwsh 'docker build -t mmohei/bmi_calculator_image .'
            }
        }
        stage("Push Docker Image"){
            steps{
                withCredentials([string(credentialsId: '', variable: 'dockerhubpwd')]) {
                    pwsh 'docker login -u mmohei -p ${dockerhubpwd}'
            }
            pwsh 'docker push mmohei/bmi_calculator_image'
            }
        }
    }
}
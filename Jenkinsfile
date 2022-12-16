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
                sh 'docker build -t mmohei/bmi_calculator_image .'
            }
        }
        stage("Push Docker Container"){
            steps{
                echo "Workspace is $WORKSPACE"
                dir("$WORKSPACE/bmi-calculator"){
                script{
                    docker.withRegistry('https://index.docker.io/v1/','DockerHub'){
                        def image = docker.build('mmohei/bmi-calculator-image:v1')
                        image.push()
                        }
                    }
                }
            }
        }
    }
}
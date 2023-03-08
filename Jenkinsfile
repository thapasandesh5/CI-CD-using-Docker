pipeline {
    agent any
    tools{
        maven 'Maven'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/thapasandesh5/devops-automation.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t thapasandesh5/devops-integration:tagname .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'Dockercredit', variable: 'Dockercredit')]) {
                   sh 'docker login -u thapasandesh5 -p ${Dockercredit}'

}
                   sh 'docker push thapasandesh5/devops-integration:tagname'
                }
            }
        }
  stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run -d -p 8008:8080 thapasandesh5/devops-integration:tagname"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://ec2-user@172.31.32.245 run -d -p 8003:8080 thapasandesh5/devops-integration:tagname"
 
            }
        }
    }
	}

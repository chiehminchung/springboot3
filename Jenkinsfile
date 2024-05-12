pipeline{
    agent any
    tools{
        maven 'maven'
    }

    stages{
        stage('build maven'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/chiehminchung/springboot3']])
//                 sh 'mvn clean install'
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('build docker image'){
                    steps{
                        script{
                            sh 'docker build -t springboot3 --platform linux/amd64 .'
//                             sh 'docker tag chiehmin/hello-eks:latest 654661216054.dkr.ecr.us-east-2.amazonaws.com/spring-eks:latest'
                            sh 'docker tag springboot3:latest 654661216054.dkr.ecr.us-east-1.amazonaws.com/springboot3:new'

                        }

                    }
        }
        stage('push into ECR'){
            steps{

                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 654661216054.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker push 654661216054.dkr.ecr.us-east-1.amazonaws.com/springboot3:new'
            }
        }
        stage('eks'){
            steps{
                // sh 'aws eks --region us-east-2 update-kubeconfig --name jam-cluster'
                sh 'kubectl apply -f mysql.yaml'
                sh 'kubectl apply -f k8s.yaml'

            }
        }
    }
}
pipeline {
    
    agent any
    
    
    environment {
        registry = "public.ecr.aws/f3l7l8x1/mrthaas-demo"
    }
    
    
    stages {
        
        stage ('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mrthaas/myPythonDockerRepo']])
            }
        }
    
    stage ('Docker Build') {
        steps {
                script {
                    dockerImage = docker.build registry
                }
            }
                
        }
        
        
        stage ('Docker Push') {
            steps {
                script {
                    sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/f3l7l8x1'
                    sh 'docker push public.ecr.aws/f3l7l8x1/mrthaas-demo:latest'

                }
            }
        }
        
        // Stopping Docker containers for cleaner Docker run
        stage('stop previous containers') {
         steps {
            sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
         }
       }
       
       stage ('Docker Run') {
           steps {
                script {
                    sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer public.ecr.aws/f3l7l8x1/mrthaas-demo:latest'
                }
           }
       }
    }
}

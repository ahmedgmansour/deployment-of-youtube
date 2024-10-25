pipeline{
    agent any
    tools{
        jdk 'jdk17'
        nodejs 'node16'
    }    
    environment {
        DOCKER_IMAGE_NAME = "ahmedgmansour/youtube"
        // NAMESPACE = " "
    }
    stages {
        stage('init') {
            steps {
                script {
                    if (env.BRANCH_NAME == "master") {
                        DOCKER_IMAGE_NAME = "ahmedgmansour/drupal"
                        // NAMESPACE = ""
                    }
                    else if (env.BRANCH_NAME == "diamond")
                    {
                        DOCKER_IMAGE_NAME = "ahmedgmansour/drupal-test"
                        // NAMESPACE = "test"
                    }
                }
            }
        }                    
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'diamond', url: 'https://github.com/Aakibgithuber/deployment-of-youtube.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }        
        stage('Compile') {
            steps {
                sh "echo hello"
            }
        }                  
        stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){   
                      sh """
                        echo ${DOCKER_IMAGE_NAME}
                        docker build -t ${DOCKER_IMAGE_NAME} .
                        docker push ${DOCKER_IMAGE_NAME}:latest1
                        """
                    }
                }
            }
        }
        stage('Deploy to container'){
            steps{
                sh 'docker run -d --name drupal -p 8080:80 ${DOCKER_IMAGE_NAME}:latest'
            }
          } 
        } 
    }

 
  

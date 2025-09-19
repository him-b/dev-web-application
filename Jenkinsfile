pipeline{
    agent any
    
    tools{
        jdk 'java-21'
        maven 'maven'
    }
    
    stages{
        stage('Git-checkout'){
            steps{
                git branch: 'dev' , url: 'https://github.com/him-b/dev-web-application.git'
            }
        }
        stage('Code Compile'){
            steps{
                sh 'mvn compile'
            }
        }
        stage('Code Package'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Build and tag'){
            steps{
                sh 'docker build -t himani708/tomcatt .'
            }
        }
        stage('Containerisation'){
            steps{
                sh '''
                docker run -it -d --name c8 -p 8080:8080  himani708/tomcatt
                '''
            }
        }
        stage('Login to Docker Hub') {
                    steps {
                        script {
                            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                                sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                            }
                        }
                    }
        }
         stage('Pushing image to repository'){
            steps{
                sh 'docker push  himani708/tomcatt'
            }
        }
        
    }
}

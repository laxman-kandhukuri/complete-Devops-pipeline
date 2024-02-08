pipeline{
    agent{
        label "jenkins-agent"
    }
    tools{
        jdk 'jdk17'
        maven 'maven3'
    }
    environment {
        APP_NAME = "complete-Devops-pipeline"
        RELEASE = "2.0.0"
        DOCKER_USER = "laxman124"
        DOCKER_PASS = 'docker'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }
        }
        stage("checkout from SCM"){
            steps {
               git branch: 'main', credentialsId: 'github', url: 'https://github.com/laxman-kandhukuri/complete-Devops-pipeline/'
            }
        }
        stage("Build application"){
            steps {
                sh "mvn clean package"
            }
        }
        stage("Test applicatoin"){
            steps {
                sh "mvn test"
         }
        }
        stage("sonarqube analysis"){
            steps {
                script {
                    echo "Workspace Path: ${WORKSPACE}"
                    withSonarQubeEnv(credentialsId: 'sonar') {
                        sh "mvn sonar:sonar"
                    }
                }
                
             
            }  
        }
        stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
        
            }
        }
    
    }
       

}

    

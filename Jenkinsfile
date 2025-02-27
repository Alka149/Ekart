pipeline {
    agent any
    tools{
        jdk 'jdk'
        maven 'maven3'
    }
    
    environment{
        SCANNER_HOME=tool 'sonar-scanner'
    }

    stages {
        stage('git checkout') {
            steps {
                git branch: 'main', changelog: false, credentialsId: 'f8e3a6a6-c59e-404c-a8a0-6f3ee021995d', poll: false, url: 'https://github.com/Alka149/Ekart.git'
            }
        }
        stage('maven compile') {
            steps {
                sh "mvn clean compile -DskipTests=true"
                
            }
        }
        
        /*
        stage('owasp scan') {
        #   steps {
        #       dependencyCheck additionalArguments: '--scan ./ ', odcInstallation: 'DP'
        #       dependencyCheckPublisher pattern: 'dependency-check-report.xml'
                
        #   }
        #}
        */
        
        stage('sonarqube') {
            steps {
                withSonarQubeEnv('sonar-server'){
                sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Ekart \
                -Dsonar.java.binaries=. \
                -Dsonar.projectKey=Ekart '''
                }
                
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn clean package -DskipTests=true"
                
            }
        }
        stage('Build and push') {
             steps {
                script{
                    withDockerRegistry(credentialsId: 'fc004b3d-17a0-44d0-b365-bccd509b91e7', toolName:'docker'){
                    sh "docker build -t ekart -f docker/Dockerfile ."
                    sh "docker tag ekart arvinda108/ekart:latest"
                    sh "docker push arvinda108/ekart:latest"
                    }
                }
             }
          }
                
     
    }
}

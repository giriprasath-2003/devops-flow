pipeline {
    agent any
    
    tools {
      nodejs 'frontend'
    }
    
     environment {
        AWS_DEFAULT_REGION = 'us-east-1'
 
        S3_BUCKET = 'frontend'
        }
    
    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'git-creds',
                    url: 'https://github.com/giriprasath-2003/frontend-1.git'
            }
         }
  
         stage('Install') {
              steps { 
                  sh 'npm install'
                }
            }   
           
         stage('Sonarqube Analysis') {
            steps {
                script {
                    def scannerhome = tool name: 'sonarqube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh """
                            ${scannerhome}/bin/sonar-scanner \
                            -Dsonar.projectKey=frontend \
                            -Dsonar.sources=frontend \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=${SONAR_TOKEN}
                            """
                 }
             }
         }
      }   
      
          stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: false
                }
            }
        }
  
          stage('build') {
              steps {
                  sh 'npm run build'
                }
             }   
        
              
                      
       }
   }

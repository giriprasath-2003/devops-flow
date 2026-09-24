pipeline {
    agent any
    
    tools {
      nodejs 'frontend'
    }
    
     environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'frontend-deploy'
        CLOUDFRONT_DIST_ID= 'E1B3IFDJXIR74Q'
        AWS_CREDENTIALS= credentials('aws-id')
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
                  dir('frontend') {
                      sh 'npm ci'
                }
            }
         }
        
         stage('build') {
              steps {
                   dir('frontend') {
                       sh 'npm run build'
                   }     
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
      
           stage('Deploy S3 Bucket'){
              steps{
                  echo 'updating S3 Bucket'
                  sh ''' 
                  aws s3 sync frontend/dist/ \
                  s3://${S3_BUCKET}/ \
                  --delete \
                  --region us-east-1
                  '''
                  echo 'Frontend Uploaded Successfully'
       }      
     }
        stage('Cloudfront Deployment'){
            steps{
                echo 'Deploying...'
                sh ''' 
                  aws cloudfront create-invalidation \
                  --distribution-id ${CLOUDFRONT_DIST_ID} \
                  --paths "/*"
                  '''
  
        }
     }
  }
} 

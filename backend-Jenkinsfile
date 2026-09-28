pipeline  {
     agent any
     
     stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'git-creds',
                    url: 'https://github.com/giriprasath-2003/backend.git'
            }
         }
         
         stage('Install Dependencies') {
            parallel {
                
                     stage('Install Auth Service') {

                    steps {

                        dir('auth-service_1784011000189') {

                            sh '''
                                echo "Installing Auth Service dependencies..."
                                npm ci
                            '''
                        }
                    }
                }


                stage('Install Task Service') {

                    steps {

                        dir('task-service') {

                            sh '''
                                echo "Installing Task Service dependencies..."
                                npm ci
                            '''
                        }
                    }
                  }
                  
                  stage('Install Notification Service') {

                    steps {

                        dir('notification-service') {

                            sh '''
                                echo "Installing Notification Service dependencies..."
                                npm ci
                            '''
                        }
                    }
                }


                stage('Install Report Service') {

                    steps {

                        dir('report-service') {

                            sh '''
                                echo "Installing Report Service dependencies..."
                                npm ci
                            '''
                        }
                    }
                }
                
                stage('Install API Gateway') {

                    steps {

                        dir('api-gateway_1784010924579') {

                            sh '''
                                echo "Installing API Gateway dependencies..."
                                npm ci
                            '''
                        }
                    }
                }
        }
      }
   }   
  }
 

pipeline {
    agent any
    
    environment {
        TOMCAT_SERVER = 'ec2-3-96-46-208.ca-central-1.compute.amazonaws.com'
    }
    
    stages {
        stage('CleanUp') {
            steps {
                sh 'rm -rf /var/lib/jenkins/workspace/java-pipeline/target/*'            
            }
        }
        
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Package') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                sh '''
                    sudo cp /var/lib/jenkins/workspace/mvn-pipeline/target/emraay-bank-app.war /opt/tomcat/webapps/
                    sudo chown tomcat:tomcat /opt/tomcat/webapps/emraay-bank-app.war
                    sudo systemctl restart tomcat
                '''
            }
        } 
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

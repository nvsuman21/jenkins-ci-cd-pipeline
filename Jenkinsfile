 
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                
                sh 'mvn clean package'
            }
            tools {
                maven 'Maven 3.6.3' 
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests...'
                
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Performing code analysis...'
                
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Scanning for security vulnerabilities...'
                
                dependencyCheck additionalArguments: '--scan ./'
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to staging environment...'
                
                sh 'aws deploy ...' // Replace with actual deployment commands
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging...'
                
                sh 'mvn verify -Dtest=IntegrationTest'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo 'Deploying to production environment...'
                
                sh 'aws deploy ...'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
        always {
            // Archive logs or artifacts if needed
        }
    }
}

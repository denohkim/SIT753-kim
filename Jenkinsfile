pipeline {
    agent any
    
    tools {
        nodejs 'Node14'  // Use Node.js 14.x, make sure to have NodeJS Plugin installed
    }

    triggers {
        pollSCM('H/5 * * * *')
    }
    
    stages {
        stage('Build') {
            steps {
                echo 'Installing dependencies'
                sh 'npm install'
                echo 'Building the application'
                sh 'npm run build'
            }
        }
        
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit tests with Jest'
                sh 'npm run test:unit'
                echo 'Running integration tests with Supertest'
                sh 'npm run test:integration'
            }
            post {
                always {
                    emailext (
                        subject: "Test Stage Status: ${currentBuild.result}",
                        body: "Test stage completed. Check console output at ${env.BUILD_URL}",
                        to: 'team@example.com',
                        attachLog: true
                    )
                }
            }
        }
        
        stage('Code Analysis') {
            steps {
                echo 'Analyzing code with ESLint'
                sh 'npm run lint'
            }
        }
        
        stage('Security Scan') {
            steps {
                echo 'Performing security scan with npm audit'
                sh 'npm audit'
            }
            post {
                always {
                    emailext (
                        subject: "Security Scan Stage Status: ${currentBuild.result}",
                        body: "Security scan completed. Check console output at ${env.BUILD_URL}",
                        to: 'team@example.com',
                        attachLog: true
                    )
                }
            }
        }
        
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to AWS EC2 staging instance using PM2'
                sh 'scp -r ./* ec2-user@staging-server:/path/to/app/'
                sh 'ssh ec2-user@staging-server "cd /path/to/app && pm2 restart app"'
            }
        }
        
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running integration tests on staging with Supertest'
                sh 'npm run test:staging'
            }
        }
        
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to AWS EC2 production instance using PM2'
                sh 'scp -r ./* ec2-user@production-server:/path/to/app/'
                sh 'ssh ec2-user@production-server "cd /path/to/app && pm2 restart app"'
            }
        }
    }
}
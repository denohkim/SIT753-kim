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
        
    stages {
        stage('Unit and Integration Tests') {
            steps {
                echo 'Running unit and integration tests'
                // Placeholder for actual test commands
                sh 'echo "Running tests..."'
            } 
            post {
                    always {
                        mail to: 'denniskimkimaiyo@gmail.com',
                             subject: "Pipeline Status: ${currentBuild.result}",
                             body: "Build ${env.BUILD_NUMBER} completed with status: ${currentBuild.result}"
                    }
            // post {
            //     always {
            //         emailext (
            //             subject: "Test Stage Status: ${currentBuild.result}",
            //             body: """
            //                 <p>Test stage has completed with status: ${currentBuild.result}</p>
            //                 <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>
            //             """,
            //             to: 'denniskimkimaiyo@gmail.com',
            //             attachLog: true,
            //             compressLog: true,
            //             mimeType: 'text/html'
            //         )
            //     }

           

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
                echo 'Performing security scan'
                // Placeholder for actual security scan commands
                sh 'echo "Running security scan..."'
            }
            post {
                always {
                    emailext (
                        subject: "Security Scan Stage Status: ${currentBuild.result}",
                        body: """
                            <p>Security scan stage has completed with status: ${currentBuild.result}</p>
                            <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>
                        """,
                        to: 'denniskimkimaiyo@gmail.com',
                        attachLog: true,
                        compressLog: true,
                        mimeType: 'text/html'
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
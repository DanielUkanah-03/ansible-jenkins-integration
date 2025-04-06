pipeline {
    agent {
        label 'amazon-linux-agent'
    }
    
    environment {
        NEXUS_URL = 'http:44.220.152.25:8081'
        NEXUS_REPO = 'devops-repo'
        NEXUS_CREDS = credentials('nexus-creds')
        APP_SERVERS = '172.31.26.110'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/DanielUkanah-03/ansible-jenkins-integration.git'
                credentialsId: 'git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Seed MongoDB') {
            steps {
                sh 'node init.js'  # Add tests if you have them
            }
        }
        
        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }
        
         stage('Package Artifact') {
            steps {
                sh 'zip -r app.zip . -x "*.git*"'
                archiveArtifacts artifacts: 'app.zip', fingerprint: true
            }
        }

      
        
        stage('Deploy with Ansible') {
            steps {
                ansiblePlaybook(
                    playbook: 'deploy.yml',
                    inventory: 'inventory.ini',
                    credentialsId: 'demo.pem',
                    colorized: true
                )
            }
        }
    }
}
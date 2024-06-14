pipeline {
    agent { 
        label 'nodejs-slave'
    }
    
    stages {
        stage('Build') {
            steps {
                checkout scm
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'dist/**/*', onlyIfSuccessful: true
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def dateTag = new Date().format('yyyyMMdd-HHmmss')
                    sh "mkdir -p /var/www/${dateTag}"
                    sh "cp -r dist/* /var/www/${dateTag}"
                    sh "ln -sfn /var/www/${dateTag} /var/www/html"
                }
            }
        }
    }
}

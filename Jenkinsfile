@Library('jenkins-shared-library') _

pipeline {
    agent { 
        label 'nodejs-slave'
    }
    
    stages {
        stage('Checkout') {
            steps {
                gitCheckout(
                    repoUrl: 'https://github.com/user/nodejs-app.git',
                    branch: 'master'
                )
            }
        }
        
        stage('Build') {
            steps {
                npmBuild()
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
                    deployApp(
                        deployDir: '/var/www',
                        tag: dateTag
                    )
                }
            }
        }
    }
}

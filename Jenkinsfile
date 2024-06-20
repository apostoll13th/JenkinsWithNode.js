@Library('my-shared-library') _

pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                gitCheckout(branch: 'main')
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

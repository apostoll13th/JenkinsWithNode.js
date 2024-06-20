pipeline {
    agent any

    def gitCheckout(String repoUrl, String branch = 'main') {
        checkout([
            $class: 'GitSCM',
            branches: [[name: "*/$branch"]], 
            userRemoteConfigs: [[url: repoUrl]]
        ])
    }

    def npmBuild() {
        sh 'npm install'
        sh 'npm run build'
    }

    def deployApp(String deployDir, String tag) {
        sh "mkdir -p ${deployDir}/${tag}"
        sh "cp -r dist/* ${deployDir}/${tag}"
        sh "ln -sfn ${deployDir}/${tag} ${deployDir}/html"
    }
    
    stages {
        stage('Checkout') {
            steps {
                gitCheckout(
                    repoUrl: 'https://github.com/apostoll13th/JenkinsWithNode.js',
                    branch: 'main'
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

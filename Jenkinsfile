def gitCheckout(String branch = 'main') {
    checkout([
        $class: 'GitSCM',
        branches: [[name: "*/$branch"]], 
        userRemoteConfigs: [[url: 'https://github.com/apostoll13th/JenkinsWithNode.js']]
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

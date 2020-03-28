pipeline {
    agent any
    stages {
        stage('Upload to AWS') {
            steps {
                withAWS(region:'us-east-1',credentials: 'aws-static') {
                    s3Upload(file:'index.html', bucket:'mansong-jenkins-udacity', path:'index.html')
                }
            }
        }
        stage('Lint HTML') {
            sh 'tidy -q -e *.html'
        }
    }
}
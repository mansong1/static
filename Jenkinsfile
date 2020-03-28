pipeline {
    agent any
    stages {
        stage('Lint HTML') {
            steps {
                sh 'tidy -q -e *.html'
            }
        }
        stage('Upload to AWS') {
            steps {
                withAWS(region:'us-east-1',credentials: 'aws-static') {
                    timeout(time: 3, unit: 'MINUTES') {
                        retry(5) {
                            s3Upload(file:'index.html', bucket:'mansong-jenkins-udacity', path:'index.html')
                        }
                    }
                }
            }
        }

    }
}
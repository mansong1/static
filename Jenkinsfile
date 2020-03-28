properties([pipelineTriggers([githubPush()])])

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
                            echo 'Uploading content with AWS creds'
                            s3Upload(pathStyleAccessEnabled:true, payloadSigningEnabled: true, file:'index.html', bucket:'mansong-jenkins-udacity')
                        }
                    }
                }
            }
        }
        stage('Post Deploy Test') {
            steps {
                echo 'Testing Deployment'
                script {
                    String webSite = 'https://mansong-jenkins-udacity.s3.us-east-1.amazonaws.com/index.html'
                    def returnCode = sh(returnStdout: true, script: "curl -sLI -o /dev/null -w '%{http_code}' ${webSite}")
                    if (returnCode != 200 && returnCode != 201) {
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }
    }
    /* Cleanup workspace */
    post {
       always {
           deleteDir()
       }
    }
}
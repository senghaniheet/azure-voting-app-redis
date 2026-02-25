pipeline{
    agent any
    stages{
        stage('Verify Branch') {
            steps {
                script {
                    echo "$GIT_BRANCH"
                }
            }
        }
        stage('Docker Build') {
            steps {
                sh(script: 'docker compose build')
            }
        }
        stage('Start App') {
            steps {
                sh(script: 'docker compose up -d')
            }
        }
        stage('Run Tests') {
            steps {
                    sh 'docker compose run --rm azure-vote-front python3 -m pytest -vv --tb=short ./tests/test-sample.py'
            }
            post {
                success {
                    echo 'Tests passed successfully! :)'
                }
                failure {
                    echo 'Tests failed. :('
                }
            }
        }
        stage('Docker push') {
            steps {
                script {
                    echo "Running in $WORKSPACE"
                    dir("$WORKSPACE/azure-vote") {
                        docker.withRegistry('', 'dockerhub') {
                            def image = docker.build("heetpatel01/azure-vote-front:${env.BUILD_NUMBER}")
                            image.push()
                        }
                    }
                }
            }
        }
        // stage('Run trivy scan') {
        //     steps {
        //         sh '''
        //             trivy image --severity HIGH,CRITICAL --exit-code 1 --no-progress heetpatel01/azure-vote-front:$BUILD_NUMBER
        //             '''
        //     }
        // }
        stage('Grype Scan') {
            steps {
                grypeScan(
                    autoInstall: true,
                    scanDest: "registry:heetpatel01/azure-vote-front:${BUILD_NUMBER}",
                    repName: "grypeReport_${JOB_NAME}_${BUILD_NUMBER}.txt"
                )
            }
        }
    }
    post {
        always {
            sh(script: 'docker compose down')
        }
    }
}
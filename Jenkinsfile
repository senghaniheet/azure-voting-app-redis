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
                sh(script: 'pytest ./tests/test_sample.py')
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
        post {
            always {
                sh(script: 'docker compose down')
            }
        }
    }
}
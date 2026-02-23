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
                sh 'docker compose build'
            }
        }
    }
}
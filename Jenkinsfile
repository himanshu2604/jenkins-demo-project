pipeline {
    agent any
    
    stages {
        stage('Pull from Git') {
            steps {
                echo 'Pulling content from Git develop branch...'
                checkout scm
                sh 'mkdir -p /var/jenkins_home/pulled_content'
                sh 'cp -r * /var/jenkins_home/pulled_content/'
                echo 'Git content successfully pulled to folder!'
            }
        }
    }
}

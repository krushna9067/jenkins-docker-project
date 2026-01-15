pipeline {
    agent any

    stage('Clone Repo') {
    steps {
        git branch: 'main',
            url: 'https://github.com/krushna9067/jenkins-docker-project.git'
    }
}

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-docker-app .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker stop myapp || true
                docker rm myapp || true
                docker run -d -p 5000:5000 --name myapp jenkins-docker-app
                '''
            }
        }
    }
}

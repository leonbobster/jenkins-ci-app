pipeline {
    agent {
        dockerfile {
            filename 'Dockerfile-docker-compose'
            dir 'docker'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'cat /etc/*-release'
                sh 'pwd'
                sh 'ls -al'
                sh 'docker build -f docker/Dockerfile-php-cli -t php56_cli .'
            }
        }
        stage('Test') {
            steps {
                sh 'docker run -i --rm --name php56-cli php56_cli php -v'
                sh 'docker run -i --rm -v `pwd`:/app -w /app --name php56-cli php56_cli composer install'
            }
        }
    }
    /*post {
        always {
        }
    }*/
}
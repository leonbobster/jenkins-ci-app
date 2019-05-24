pipeline {
    agent {
        dockerfile {
            filename 'Dockerfile-docker-compose'
            dir 'docker'
        }
    }
    stages {
        stage('Test') {
            steps {
                wrap([$class: 'AnsiColorBuildWrapper', 'colorMapName': 'xterm']) {
                    sh 'docker-compose -f docker/test.yml up -d'
                    sh 'docker-compose -f docker/test.yml run cp-app-cli php -v'
                    sh 'docker-compose -f docker/test.yml run cp-app-cli composer install'
                    sh 'docker-compose -f docker/test.yml run cp-app-cli ./vendor/bin/codecept run unit,acceptance'
                }
            }
        }
    }
    post {
        always {
            sh 'docker-compose -f docker/test.yml down'
        }
    }
}
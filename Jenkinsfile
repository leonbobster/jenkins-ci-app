void setBuildStatus(String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/leonbobster/jenkins-ci-app"],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}

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
                setBuildStatus("Build succeeded", "PENDING");
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
        success {
            setBuildStatus("Build succeeded", "SUCCESS");
        }
        failure {
            setBuildStatus("Build failed", "FAILURE");
        }
    }
}
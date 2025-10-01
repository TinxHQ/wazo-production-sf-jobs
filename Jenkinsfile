pipeline {
  agent any
  options {
    disableConcurrentBuilds()
    timeout(time: 5, unit: 'MINUTES')
    buildDiscarder(logRotator(numToKeepStr: '100'))
    timestamps()
  }

  stages {
    stage ('Prepare') {
      steps {
        checkout scm
      }
    }
    stage ('Verify') {
      steps {
        sh 'grep "pipx install" roles/ensure-ansible-lint/tasks/main.yaml'
      }
    }
  }
  post {
    failure {
      mattermostSend color: "danger", channel: "#infrastructure", message: "[Software Factory jobs](https://github.com/TinxHQ/wazo-production-sf-jobs) is missing bookworm steps on master - [Job :nuke:](${JOB_URL})"
    }
  }
}

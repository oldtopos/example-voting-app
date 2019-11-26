pipeline {
  agent {
    node {
      label 'ubuntu1804'
    }
  }
  stages {
    stage('Build result') {
      steps {
        sh 'docker build -t dockersamples/result ./result'
      }
    } 
    stage('Build vote') {
      steps {
        sh 'docker build -t dockersamples/vote ./vote'
      }
    }
    stage('Build worker') {
      steps {
        sh 'docker build -t dockersamples/worker ./worker'
      }
    }
    stage('Push result image') {
      when {
        branch 'master'
      }
      steps {
        docker.withRegistry('https://888283091142.dkr.ecr.us-west-2.amazonaws.com/dockersamples') {
          docker.image('dockersamples:result').push('result')
        }
      }
    }
    stage('Push vote image') {
      when {
        branch 'master'
      }
      steps {
        docker.withRegistry('https://888283091142.dkr.ecr.us-west-2.amazonaws.com/dockersamples') {
          docker.image('dockersamples:vote').push('vote')
        }
      }
    }
    stage('Push worker image') {
      when {
        branch 'master'
      }
      steps {
        docker.withRegistry('https://888283091142.dkr.ecr.us-west-2.amazonaws.com/dockersamples') {
          docker.image('dockersamples:worker').push('worker')
        }
      }
    }
  }
}

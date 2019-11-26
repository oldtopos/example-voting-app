  node( 'ubuntu1804' ) {
    stage('Checkout' ) {
      checkout scm
    }
    stage('Build result') {
        sh 'docker build -t dockersamples:result ./result'
    } 
    stage('Build vote') {
        sh 'docker build -t dockersamples:vote ./vote'
    }
    stage('Build worker') {
        sh 'docker build -t dockersamples:worker ./worker'
    }
    stage('Push result image') {
     try {
       timeout(30) {
         docker.withRegistry('https://888283091142.dkr.ecr.us-west-2.amazonaws.com/dockersamples') {
           docker.image('dockersamples:result').push('result')
         }
       }
     } catch (e) {
         notify_user("Docker stack push for result failed", red)
         throw e
     }
    }
    stage('Push vote image') {
        docker.withRegistry('https://888283091142.dkr.ecr.us-west-2.amazonaws.com/dockersamples') {
          docker.image('dockersamples:vote').push('vote')
        }
    }
    stage('Push worker image') {
        docker.withRegistry('https://888283091142.dkr.ecr.us-west-2.amazonaws.com/dockersamples') {
          docker.image('dockersamples:worker').push('worker')
        }
    }
    stage('SSH transfer') {
      script {
        sshPublisher(
          continueOnError: false, failOnError: true,
          publishers: [
            sshPublisherDesc(
              configName: "Docker Swarm Manager",
              verbose: true,
              transfers: [
                sshTransfer(
                  sourceFiles: "./docker-stack.yml, ./docker-stack.yml",
                  removePrefix: ".",
                  remoteDirectory: "/home/docker",
                  execCommand: "docker stack deploy --compose-file docker-stack.yml vote"
                )
             ])
         ])
       }
     }
  }

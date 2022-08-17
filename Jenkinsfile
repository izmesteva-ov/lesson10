pipeline {
    agent {
       docker {
         image '51.250.41.105:8082/jenkins-agent'
         args '-u root -v /var/run/docker.sock:/var/run/docker.sock'
    }
  }
  stages {
    stage('git clone') {
        steps {
             git 'https://github.com/izmesteva-ov/original11.git'
         } 
      }
    stage('Build war') {
        steps {
           sh 'mvn package'
      }
    }

}
  }






  

  


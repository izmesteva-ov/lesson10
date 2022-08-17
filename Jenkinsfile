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
    stage('Docker image') {
        steps {
          sh 'docker build -t myapp .'
          sh 'docker login -u admin -p admindemo 51.250.41.105:8082'
          sh 'docker tag myapp 51.250.41.105:8082/myapp'
          sh 'docker push 51.250.41.105:8082/'
       }
    }
  
  }
  
  }

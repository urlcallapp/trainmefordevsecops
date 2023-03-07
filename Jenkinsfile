pipeline {
  agent {
	label 'docker'
} 
   stages {
    stage('Cloning Git') {
      steps
      {
        /* Let's make sure we have the repository cloned to our workspace */
        //checkout scmGit(branches: [[name: '*/email-notification']], extensions: [], userRemoteConfigs: [[credentialsId: 'ssh_gitlab_key', url: 'git@gitlab.com:urlcallapp/trainmefordevsecops.git']])
        sh 'git clone from SCM inside jenkins'
      }
    }
    stage('SAST - SonarQube Analysis') {
      steps
      {
        sh 'echo SAST stage with SonarQube'        
        //def scannerHome = tool 'SonarScanner';
        //withSonarQubeEnv() {
        //  sh "${scannerHome}/bin/sonar-scanner"
        //}
      }
    }    
    stage('Build-and-Tag') {
    /* This builds the actual image; synonymous to
         * docker build on the command line */
      steps{   
        script{
          app = docker.build("urlcallapp/snake:${env.BUILD_ID}")
        } 
        sh 'echo Build and Tag'
      }
    }
    stage('Post-to-dockerhub') {
      steps
      {
        script {
            docker.withRegistry('https://registry.hub.docker.com','dockerhub') {            
            app.push()
            app.push('latest')
          }
        }
        sh 'echo post to dockerhub repo'
      }
    }
    stage('SECURITY-IMAGE-SCANNER'){
      steps 
      {
        sh 'echo scan image for security'
      }
    }
    stage('Pull-image-server') {
      steps
      {
         sh 'echo pulling image ...'
         sh 'docker rm -f trainmefordevsecops_snake_1'
         sh 'docker-compose up -d'
      }
    }    
    stage('DAST') {
      steps
      {
         sh 'echo dast scan for security'
      }
    }
  }
}

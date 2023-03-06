pipeline {
agent { label 'docker' }

   stages {

    stage('Cloning Git') {

      steps
        {
        /* Let's make sure we have the repository cloned to our workspace */
          sh ''
        }  
    }
    stage('SAST'){
      steps{
        sh 'echo SAST stage'
       }
    }

    
    stage('Build-and-Tag') {
    /* This builds the actual image; synonymous to
         * docker build on the command line */
      steps{    
        sh 'echo Build and Tag'
        script {
          app= docker.build("sunsdbook13/snake:${env.BUILD_ID}")
        }
      }
    }

    stage('Post-to-dockerhub') {
     steps {
        sh 'echo post to dockerhub repo'
     }
    }

    stage('SECURITY-IMAGE-SCANNER'){
      steps {
        sh 'echo scan image for security'
     }
    }

    stage('Pull-image-server') {
      steps {
         sh 'echo pulling image ...'
       }
      }
    
    stage('DAST') {
      steps  {
         sh 'echo dast scan for security'
        }
    }
 }
}
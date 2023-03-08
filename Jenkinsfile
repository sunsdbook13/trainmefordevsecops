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
      environment {
          SONAR_SCANNER_VERSION = '4.7.0.2747'
          SONAR_SCANNER_HOME = "$HOME/.sonar/sonar-scanner-$SONAR_SCANNER_VERSION-linux"
          PATH = "$SONAR_SCANNER_HOME/bin:$PATH"
          SONAR_SCANNER_OPTS = '-server'
          SONAR_TOKEN='a62624f06c8581026c7316057179e8db8c5eb75c'
      }
      steps{
          sh  'curl --create-dirs -sSLo $HOME/.sonar/sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-$SONAR_SCANNER_VERSION-linux.zip'
          sh  'unzip -o $HOME/.sonar/sonar-scanner.zip -d $HOME/.sonar/'
          sh  '''sonar-scanner \
              -Dsonar.organization=sunsdbook13 \
              -Dsonar.projectKey=sunsdbook13_snake \
              -Dsonar.sources=. \
              -Dsonar.host.url=https://sonarcloud.io'''
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
        script {
          docker.withRegistry('https://registry.hub.docker.com','docker-hub') {
            app.push("${env.BUILD_ID}")
          }
        }
      }
    }

    stage('SECURITY-IMAGE-SCANNER'){
      steps {
        sh 'echo scan image for security'
        sh "trivy image sunsdbook13/snake:${env.BUILD_ID}"
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
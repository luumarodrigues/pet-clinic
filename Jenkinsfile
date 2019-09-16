pipeline {
  agent { label 'master' }

  stages {
    stage('Clean WorkSpace') {
      steps {
        sh 'echo -e "\033[0;34m## Limpando o Workspace  ##\033[0m"'
        deleteDir()
      }
    }

    stage('git clone') {
      steps{
          sh 'echo -e "\033[0;34m## Clonando repositório ##\033[0m"'
          checkout([
            $class: 'GitSCM',
            extensions: [
              [$class: 'CleanCheckout'],
            ],
            branches: [
              [name: 'master']
            ],
            userRemoteConfigs:
              [[
                credentialsId: '',
                url: 'https://github.com/skraelings888/pet-clinic.git'
              ]]
            ])
         }
      }

    stage('Build da aplicação') {
      steps {
        sh 'echo -e "\033[0;33m## Build ##\033[0m"'
          sh '''
             /usr/local/src/apache-maven/bin/mvn clean install
             '''
        }
      }

    stage('Disponibilizando artefato para Download dentro do Job') {
        steps {
          sh 'echo -e "\033[0;34m## Archiving artifacts  ##\033[0m"'
          archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
          }
        }

    stage('Disponibilizando report Junit dentro do Job') {
        steps {
          sh 'echo -e "\033[0;34m## Junit report ##\033[0m"'
            junit '**/target/surefire-reports/*.xml'

            }
          }
        }
      }


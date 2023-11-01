pipeline {
   agent any
   options {
      ansiColor('xterm')
   }
   parameters {
      string(name: 'COMPONENT', defaultValue: '', description: 'Enter component name')
      string(name: 'ENV', defaultValue: 'dev', description: 'Which Environment it is')
      string(name: 'APP_VERSION', defaultValue: '', description: 'Which App version to deploy')
    }

   stages {
      stage('Get Kubeconfig') {
         steps {
            sh 'aws eks update-kubeconfig --name ${ENV}-roboshop'
         }
      }
      stage('Get APP code') {
         steps {
            dir('APP') {
               git branch: 'main', url: 'https://github.com/hemanthtadikonda/${COMPONENT}.git'
            }
         }
      }
      stage('Get Helm code') {
         steps {
            dir('CHART') {
               git branch: 'main', url: 'https://github.com/hemanthtadikonda/roboshop-helm.git'
            }
         }
      }
      stage('Helm Deploy') {
         steps {
            //sh 'helm install ${COMPONENT} ./CHART -f /APP/helm/${ENV}.yaml --set APP_VERSION=${APP_VERSION}'
            sh 'helm upgrade -i ${COMPONENT} ./CHART -f APP/helm/${ENV}.yaml --set app_version=${APP_VERSION}'
         }
      }
   }
}

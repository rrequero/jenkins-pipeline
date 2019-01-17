#!groovy

def jenkinsStage = "${JENKINS_STAGE}"
def branch = "${env.BRANCH_NAME}"

println "Running Jenkins Pipeline"
println "SCENARIO -> Stage: ${jenkinsStage} // Branch: ${branch}"

pipeline {
  agent {
    kubernetes {
      label 'builder-agent'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: jenkins
  volumes:
  - name: container-builder-key
    secret:
      secretName: container-builder-key
  containers:
  - name: jnlp
    env:
    - name: JENKINS_URL
      value: http://jenkins
  - name: node
    image: node:10.7.0
    command:
    - cat
    tty: true
  - name: kubectl
    image: gcr.io/cloud-builders/kubectl
    command:
    - cat
    tty: true
"""
}
  }
  stages {
    
   
    stage('3 - Deploy') {
      when {
        anyOf {
          allOf {
            expression { branch == 'develop'}
          }
          allOf {
            expression { branch == 'master'}
          }
        }
      }
      steps{
        script {
            stage ('3a - Deploying to Staging') {
              container('kubectl') {
              
                
                sh("kubectl get pods")
                
              }
            }
        }
          
        
      }
    }
  }
}

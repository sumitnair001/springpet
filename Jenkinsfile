pipeline{  
    environment {
    registry = "sumitnair89/ct-wordpress"
    }
  agent any
  stages {
      
      stage('install required packages') {
          
           steps {
               sh "/usr/bin/python2.7 -m pip install --upgrade --user openshift"
               sh "/usr/bin/python2.7 -m pip install Kubernetes"

           }
          }
     
           stage('Build') {
          
           steps {
                
                   sh 'pwd'
                   sh 'docker build .'
                      }
       }
       
      
       stage('Publish') {
           environment {
               registryCredential = 'dockerhub'
           }
           steps{
              
              script {
                 
                  docker.withRegistry( '', registryCredential ) {
                      def appimage = docker.build registry + ":$BUILD_NUMBER"
                      appimage.push()
                      
                  }
              }
           }
       }
       stage ('Deploy') {
           steps {
               script{
                   def image_id = registry + ":$BUILD_NUMBER"
                   sh "sed -i 's|image_id|$image_id|g' deployment.yml"
                   sh "ansible-playbook  playbook.yml"
                }
           }
       }
   }
}




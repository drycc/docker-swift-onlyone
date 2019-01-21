#!groovy

node('linux') {
 stage 'Checkout'
 checkout scm

 stage 'Docker Build and Push - Quay.io'
 def quayUsername = "drycc+jenkins"
 def quayEmail = "drycc+jenkins@drycc.com"
 withCredentials([[$class: 'StringBinding',
                    credentialsId: '8317a529-10f7-40b5-abd4-a42f242f22f0',
                    variable: 'QUAY_PASSWORD']]) {

   sh """
     docker login -e="${quayEmail}" -u="${quayUsername}" -p="\${QUAY_PASSWORD}" quay.io
     make build push
   """
 }

 stage 'Docker Build and Push - DockerHub'
 def hubUsername = "dryccbot"
 def hubEmail = "dummy-address@drycc.com"
 withCredentials([[$class: 'StringBinding',
                    credentialsId: '0d1f268f-407d-4cd9-a3c2-0f9671df0104',
                    variable: 'DOCKER_PASSWORD']]) {

   sh """
    docker login -e="${hubEmail}" -u="${hubUsername}" -p="\${DOCKER_PASSWORD}"
    REGISTRY='' make build push
   """
 }
}

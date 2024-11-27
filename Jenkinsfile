pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded 
            }
        } 

      stage('unit test') {
        steps {
              sh "mvn test"
             }
        post { 
          always { 
             junit 'target/surefire-reports/*.xml'
            jacoco execPattern: 'target/jacoco.exec'
          }
       }
     }

     stage('Docker Build and Push') {
            steps {
              withDockerRegistry([credentialsId: "docker", url:""]) {
              sh 'printenv'
              sh 'echo $GIT_COMMIT'
              sh 'docker build -t adarijaganadha/numerica-app:"$GIT_COMMIT" .'
              sh 'docker push adarijaganadha/numerica-app:"$GIT_COMMIT"'
            }
          }
      }

       stage('K8S Deployment - DEV') {
       steps {
          withKubeConfig([credentialsId: 'kubeconfig']) {
               sh "sed -i 's#replace#adarijaganadha/numerica-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
               sh "kubectl -n default apply -f k8s_deployment_service.yaml"
             }
           }
    }
}



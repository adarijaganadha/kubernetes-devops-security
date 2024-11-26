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
         }
      post { 
       always { 
          junit 'target/reports/*.xml'
           jacoco execPattern: 'target/jacoco.exec'
       }
      }
    }
}

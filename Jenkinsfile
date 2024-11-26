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
              sh 'printenv'
              sh 'docker build -t numerica-app:""$GIT COMMIT"" .'
              SH 'docker push numerica-app:""$GIT COMMIT""'
            }
        }
    }
}

pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar'
            }
        }
      stage('Unit Test') {
            steps {
              sh "mvn test"
            }
        }
      stage('Docker Build and Push') {
            steps {
              withDockerRegistry([credentialsID: "docker-hub", url: ""]){
                  sh 'printenv'
                  sh 'docker build -t heycharles/numeric-app:""$GIT_COMMIT"" .'
                  sh 'docker push heycharles/numeric-app:""$GIT_COMMIT""'
              }
              
            }
        }
    }
}
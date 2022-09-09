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
              withDockerRegistry([credentialsId: "docker-hub", url: ""]){
                  sh 'printenv'
                  sh 'docker build -t heycharles/numeric-app:""$GIT_COMMIT"" .'
                  sh 'docker push heycharles/numeric-app:""$GIT_COMMIT""'
              }
              
            }
        }
      stage('Kubernetes Deployment - DEV') {
            steps {
              withKubeConfig([credentialsId: 'kubecon']) {
              sh "sed -i 's#replace#heycharles/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
              sh "kubectl apply -f k8s_deployment_service.yaml"
              }
              
            }
        }
    }
}
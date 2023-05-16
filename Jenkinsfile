node {
  stage("Clone the project") {
    git branch: 'main', credentialsId: 'git-id', url: 'https://github.com/vishalkhillare/jenkins-demo-springboot.git'
  }

  stage("Compilation") {
    sh "./mvnw clean install -DskipTests"
  }

  stage("Tests and Deployment") {
    stage("Runing unit tests") {
      sh "./mvnw test -Punit"
    }
    stage("Deployment") {
      sh 'nohup ./mvnw spring-boot:run -Dserver.port=8001 &'
    }
  }
}

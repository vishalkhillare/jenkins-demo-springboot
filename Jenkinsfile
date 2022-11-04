node {
    stage 'Clone the project'
    git 'https://github.com/eugenp/tutorials.git'

    dir('spring-jenkins-pipeline') {
        stage("Compilation") {
            sh "./mvnw clean install -DskipTests"
        }

        stage("Tests and Deployment") {
            parallel 'Unit tests': {
                stage("Runing unit tests") {
                    try {
                        sh "./mvnw test -Punit"
                    } catch(err) {
                        step([$class: 'JUnitResultArchiver', testResults:
                          '**/target/surefire-reports/TEST-*UnitTest.xml'])
                        throw err
                    }
                   step([$class: 'JUnitResultArchiver', testResults:
                     '**/target/surefire-reports/TEST-*UnitTest.xml'])
                }
            }

            stage("Deployment") {
                sh "pid=\$(lsof -i:8001 -t); kill -TERM \$pid "
                  + "|| kill -KILL \$pid"
                withEnv(['JENKINS_NODE_COOKIE=dontkill']) {
                    sh 'nohup ./mvnw spring-boot:run -Dserver.port=8001 &'
                }
            }
        }
    }
}
node {
    checkout scm
    docker.image('maven:3.8.1-adoptopenjdk-11')
        .inside('-v /root/.m2:/root/.m2') {
            stage('Build') {
                sh 'mvn -B -DskipTests clean package'
            }
        }
}
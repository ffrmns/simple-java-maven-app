node {
    checkout scm
    docker.image('maven:3.8.1-adoptopenjdk-11')
        .inside('-v /root/.m2:/root/.m2') {
            stage('Build') {
                sh 'mvn -B -DskipTests clean package'
            }
            try {
                stage('Test') {
                    sh 'mvn test'
                }
            } finally {
                junit 'target/surefire-reports/*.xml'
            }
            stage('Manual Approval') {
                input(message : "Lanjutkan ke tahap Deploy?")
            }
            stage('Deploy') {
                sh './jenkins/scripts/deliver.sh'
                sleep 60
            }
        }
}
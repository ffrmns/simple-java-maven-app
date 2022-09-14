node {
    checkout scm
    properties([
        skipStagesAfterUnstable(),
    ])
    checkout skipStagesAfterUnstable
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
            stage('Deliver') {
                sh './jenkins/scripts/deliver.sh'
            }
        }
}
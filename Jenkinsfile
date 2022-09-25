node {
    checkout scm
    def environment = docker.build 'maven-heroku'
    environment.inside('-v /root/.m2:/root/.m2') {
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
                sh 'heroku git:remote -a simple-java-maven-app'
                sh '''
                    git config user.email "massfikri@yahoo.co.id"
                    git config user.name "Fikri Firmansyah Akbar"
                '''
                sh 'git add .'  
                sh 'git commit -m "push with jenkins"'
                sh 'git push -f https://heroku:f273a98d-24e2-4014-a9ec-f1afab0ffe81@git.heroku.com/simple-java-maven-app.git HEAD:refs/heads/master'
            }
        }
}
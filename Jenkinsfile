pipeline {
    agent any
    stages {
        stage('Build with Gradle Wrapper') {
            steps {
                bat './gradlew build'
                archiveArtifacts artifacts: 'build/libs/**', allowEmptyArchive: true
            }
        }
        stage('Publish to Maven Repository') {
            steps {
                bat './gradlew publish'
            }
        }
    }
    post {
        always {
            script {
                def status = currentBuild.result ?: 'SUCCESS'
                mail to: "${env.MAIL_TO}",
                     subject: "OGL TP7 - Java CI with Gradle - ${status}",
                     body: "Status: ${status}"
            }
        }
    }
}

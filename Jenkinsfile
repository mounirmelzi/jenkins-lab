pipeline {
    agent any
    stages {
        stage('Run Unit Tests') {
            steps {
                bat './gradlew test'
                archiveArtifacts artifacts: 'build/reports/tests/**', allowEmptyArchive: true
            }
        }
        stage('Generate Cucumber Reports') {
            steps {
                bat './gradlew generateCucumberReports'
                archiveArtifacts artifacts: 'build/reports/cucumber/**', allowEmptyArchive: true
            }
        }
        stage('Generate JaCoCo Coverage Report') {
            steps {
                bat './gradlew jacocoTestReport'
                archiveArtifacts artifacts: 'build/reports/jacoco/**', allowEmptyArchive: true
            }
        }
        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('sonar') {
                    bat './gradlew sonar'
                }
            }
        }
        stage('SonarQube Quality Gate Check') {
            steps {
                script {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Build with Gradle Wrapper') {
            steps {
                bat './gradlew build'
                archiveArtifacts artifacts: 'build/libs/**', allowEmptyArchive: true
            }
        }
        stage('Generate Documentation') {
            steps {
                bat './gradlew javadoc'
                archiveArtifacts artifacts: 'build/docs/javadoc/**', allowEmptyArchive: true
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
                slackSend(
                    color: status == 'SUCCESS' ? 'good' : 'danger',
                    message: "OGL TP7 - Java CI with Gradle. Status: ${status}."
                )
                mail to: "${env.MAIL_TO}",
                     subject: "OGL TP7 - Java CI with Gradle - ${status}",
                     body: "Status: ${status}"
            }
        }
    }
}

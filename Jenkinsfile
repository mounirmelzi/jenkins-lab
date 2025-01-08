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
}

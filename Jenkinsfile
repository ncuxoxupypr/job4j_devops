pipeline {
    agent any

    stages {

        stage('Build') {
            parallel {

                stage('Checkstyle Main') {
                    steps {
                        echo 'Checkstyle Main'
                        sh './gradlew checkstyleMain'
                    }
                }

                stage('Checkstyle Test') {
                    steps {
                        echo 'Checkstyle Test'
                        sh './gradlew checkstyleTest'
                    }
                }

                stage('Build') {
                    steps {
                        echo 'Build'
                        sh './gradlew compileJava'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Test'
                sh './gradlew test'

                echo 'JaCoCo Report'
                sh './gradlew jacocoTestReport'

                echo 'JaCoCo Verification'
                sh './gradlew jacocoTestCoverageVerification'
            }
        }
    }
}

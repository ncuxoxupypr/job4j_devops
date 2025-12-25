pipeline {
    agent { label 'agent1' }

    tools {
        git 'Default'
    }

    stages {
        stage('Prepare Environment') {
            steps {
                sh 'chmod +x ./gradlew'
            }
        }
        stage('Build') {
            parallel {
                stage('Checkstyle Main') {
                    steps {
                        sh './gradlew checkstyleMain'
                    }
                }
                stage('Checkstyle Test') {
                    steps {
                        sh './gradlew checkstyleTest'
                    }
                }
                stage('Compile') {
                    steps {
                        sh './gradlew compileJava'
                    }
                }
            }
        }
        stage('Test & Coverage') {
            steps {
                sh './gradlew test'
                sh './gradlew jacocoTestReport'
                sh './gradlew jacocoTestCoverageVerification'
            }
        }
    }
    post {
        always {
            script {
                def buildInfo =
                        "Build number: ${currentBuild.number}\n" +
                        "Build status: ${currentBuild.currentResult}\n" +
                        "Started at: ${new Date(currentBuild.startTimeInMillis)}\n" +
                        "Duration so far: ${currentBuild.durationString}"

                telegramSend(message: buildInfo)
            }
        }
    }
}

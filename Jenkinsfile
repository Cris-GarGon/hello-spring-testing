pipeline {
    agent any
    stages{ 
        stage('Build') {
            steps {
                withGradle {
                    sh './gradlew assemble'
                }
            }
            post {
               success{
                  archiveArtifacts 'build/libs/*.jar'
               }   
            }
        }

        stage('Test') {
            steps {
                withGradle {
                    sh './gradlew clean test'
                    sh './gradlew check'
                }
            }
            post {
                always {
                    junit 'build/test-results/test/TEST-*.xml'
                    
                    )

                }
            }
        }

        stage('QA') {
            steps {

            }
            post {
                always {
                    recordIssues(
                        enabledForFailure: true, 
                        tool: pmdParser(pattern: 'build/reports/pmd/*.xml')
                }
            }
    }
}

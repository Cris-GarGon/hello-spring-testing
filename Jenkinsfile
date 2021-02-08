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
                    //sh './gradlew clean pitest'
                    sh './gradlew check'
                }
            }
            post {
                always {
                    junit 'build/test-results/test/TEST-*.xml'
                    //pitmutation mutationStatsFile: 'build/reports/pitest/**/*.xml'
                    recordIssues(
                        enabledForFailure: true, 
                        tool: pmd(pattern: 'build/reports/pmd/*.xml')
                    )

                }
            }
        }
    }
}

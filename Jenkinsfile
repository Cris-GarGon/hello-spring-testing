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
                    sh './gradlew clean pintest'
                }
            }
            post {
                always {
                    junit 'build/test-results/test/TEST-*.xml'
                    pitmutation mutationStatsFile: 'build/reports/**/mutations.xml'
                }
            }
        }
    }
}

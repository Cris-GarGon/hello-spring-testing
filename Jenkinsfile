pipeline {
    agent any
    tools {
        jdk 'OpenJDK-15.0.2'
    }
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
                    sh './gradlew clean pitest'
                }
            }
            post {
                always {
                    junit 'build/test-results/test/TEST-*.xml'
                    pitmutation mutationStatsFile: 'build/reports/pitest/**/*.xml'
                }
            }
        }
    }
}

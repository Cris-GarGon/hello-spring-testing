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
                }
            }
            post {
                always {
                    junit 'build/test-results/test/TEST-*.xml'
                }
            }
        }

        stage('pitest') {
            steps {
                withGradle {
                    sh './gradlew clean pitest'
                }
            }
            post {
                always {
                    pitmutation mutationStatsFile: 'build/reports/pitest/**/mutations.xml'
                }
            }
        }
    }
}

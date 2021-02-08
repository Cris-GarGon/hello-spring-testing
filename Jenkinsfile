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
                    
                    configFileProvider([configFile(
                    fileId:'SonarQube-gradle.properties',
                    targetLocation: 'gradle.properties')]) {
                        sh './gradlew sonarqube'
                    }

                }
            }
            post {
                always {
                    junit 'build/test-results/test/TEST-*.xml'

                }
            }
        }

        stage('QA') {
            steps {
                sh './gradlew check'
            }
            post {
                always {
                    recordIssues(
                        enabledForFailure: true, 
                        tool: spotBugs(pattern: 'build/reports/spotbugs/*.xml')
                    )
                }
            }
        }
    }
}

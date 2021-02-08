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
                    
                    configFileProvider([configFile(fileId: 'e9e5e453-9880-408a-98b8-b3c0a2346816', targetLocation: 'gradle.properties')]) {
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

                withSonarQubeEnv(credentialsID: 'ba6547cc-5244-48a3-b921-0a4361ec46a4', installationName: 'local') { 
                    sh './gradlew sonarqube'
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

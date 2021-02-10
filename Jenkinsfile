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

                  withCredentials([string(
                        credentialsId: 'gitLabPrivateToken',
                        variable: 'TOKEN')]) {
                            sh './gradlew publish'
                    } 
 
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

        stage('Segurity') {
            steps {
                sh './gradlew dependencyCheckUpdate'
                sh './gradlew dependencyCheckAnalyze'
            }
            post {
                always {
                    dependencyCheckPublisher pattern:'build/reports/dependency-check-report.xml'
                }
            }
        }
    }
}

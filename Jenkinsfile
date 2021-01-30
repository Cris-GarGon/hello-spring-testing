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
                }
            }
            post {
                always {
                    junit 'build/test-results/test/TEST-*.xml'
                    jacoco( 
                        execPattern: 'build/jacoco/*.exec',
                        classPattern: 'build/classes',
                        sourcePattern: 'src/main/java',
                        exclusionPattern: 'src/test*')
                }
            }
        }
    }
}

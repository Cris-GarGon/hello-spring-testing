pipeline {
    agent any
    tools {
        jdk 'OpenJDK-15.0.2'
    }

    stages {
        stage('Setup') {
            steps {
                sh 'echo "JAVA_HOME=${JAVA_HOME}"'
            }
        }
        
        stage('Build') {
            steps {
                withGradle {
                    sh './gradlew assemble'
                }
            }
        }
        
        stage('Test'){
            steps{
                withGradle {
                    sh './gradlew test'
                }
            }
        }
    }
    
}

pipeline {
    agent any
        
        stage('Build') {
            steps {
                withGradle {
                    sh './gradlew assemble'
                }
            }
        }
    }
    
}

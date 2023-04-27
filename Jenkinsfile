pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {

        stage('Git') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/jglick/simple-maven-project-with-tests.git'
                }
            }

        stage('Compile'){
            steps {
                sh "mvn clean compile"
            }
        }

        stage('Test') {
            steps { 
                sh "mvn test"
            }
            post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'  
                }
            }
        }

        stage('Package') {
            steps {
                sh "mvn test -DskipTests -Dmaven.test.skip package"
            }
            post {
                success {
                    archiveArtifacts 'target/*.jar'
                }
            }
        }   
    }
}


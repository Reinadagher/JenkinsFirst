pipeline {
    agent any

    parameters {
        
        string(name: 'URL', defaultValue: '', description: 'URL Java Projet to Build')
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch')
    }

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {

        stage('Git') {
            steps {
                // Get some code from a GitHub repository
                git url: "${params.URL}",
                    branch: "${params.BRANCH}"
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
                    archiveArtifacts 'target/*.jar, target/*.war'
                }
            }
        }   
    }
}


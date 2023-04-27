pipeline {
    agent any

    parameters {
        
        string(name: 'URL', defaultValue: '', description: 'URL Java Projet to Build')
        string(name: 'BRANCH', defaultValue: 'main', description: 'Branch')
        choice(name: 'JAVA_Version', choices:['17','11','20','21','22'], description: 'Java version to use')
    }

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
        jdk "JDK-${params.Java_Version}"
    }

    stages {

        stage('Git') {
            steps {
                // Get some code from a GitHub repository
                git url: "${params.URL}",
                    branch: "${params.BRANCH}"
                sh "sed -i -e 's@<maven.compiler.target>.*</maven.compiler.target>@<maven.compiler.target>${params.Java_Version}</maven.compiler.target>@' -e 's@<maven.compiler.source>.*</maven.compiler.source>@<maven.compiler.source>${params.Java_Version}</maven.compiler.source>@' pom.xml"
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


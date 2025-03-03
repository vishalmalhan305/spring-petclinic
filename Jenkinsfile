pipeline {
    agent any

    triggers {
        cron('H/3 * * * 1') // Runs every 3 minutes on Monday
    }

    environment {
        MAVEN_HOME = "C:\\maven"
        PATH = "${MAVEN_HOME}\\bin;${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/vishalmalhan305/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Test & Coverage') {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    jacoco(execPattern: '**/target/site/jacoco/jacoco.xml')
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    } // **Make sure this closing brace is correctly placed**

    post {
        success {
            echo "Pipeline executed successfully!"
        }
    }
}

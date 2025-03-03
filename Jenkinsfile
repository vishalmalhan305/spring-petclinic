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
        }

        stage('Code Coverage Report') {
            steps {
                jacoco execPattern: '**/target/jacoco.exec'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
            archiveArtifacts artifacts: '**/target/jacoco.exec', fingerprint: true
        }

        success {
            echo "Pipeline executed successfully!"
        }
    }
}

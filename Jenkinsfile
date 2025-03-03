pipeline {
    agent any

    triggers {
        cron('H/3 * * * 1') // Runs every 3 minutes on Monday
    }

     environment {
        MAVEN_HOME = "C:\\maven" // Windows Maven Path
        PATH = "${MAVEN_HOME}\\bin;${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/vishalmalhan305/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test & Coverage') {
            steps {
                sh 'mvn test'
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
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
    }
}

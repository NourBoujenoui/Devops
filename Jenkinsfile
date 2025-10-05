pipeline {
    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven'
    }

    environment {
        SONARQUBE_SERVER = 'sonarqube'
        SONARQUBE_TOKEN = 'squ_c98b55925239c3d49982fcdb9ece86c129bc8106'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/NourBoujenoui/Devops.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(SONARQUBE_SERVER) {
                    // Use -Dsonar.login to provide the token directly
                    sh "mvn sonar:sonar -Dsonar.login=${SONARQUBE_TOKEN}"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                // Add timeout to avoid indefinite IN_PROGRESS hang
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deployment step - customize as needed'
            }
        }
    }

    post {
        success {
            echo 'Build, test, and analysis completed successfully!'
        }
        failure {
            echo 'Build or analysis failed.'
        }
    }
}

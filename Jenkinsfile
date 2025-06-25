pipeline{
    agent {label 'Jenkins-agent'}
    tools {
        maven 'Maven3'
        jdk 'Java17'
    }
    stages{
        stage("Clean Workspace") {
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/AmmarAlshari/register-app.git'
            }
        }
        stage("Build Application") {
            steps {
                sh 'mvn clean package'
            }
        }
        stage("Test Application") {
            steps {
                sh 'mvn test'
            }
        }
        stage("SonarQube Analysis") {
            steps {
               script {
                    withSonarQubeEnv(credentialsID: 'jenkins-sonarqube-token') {
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }
        stage("Quality Gate") {
            steps {
                script {
                        waitForQualityGate abortPipeline: false, credentialsID: 'jenkins-sonarqube-token'
                    }
                }
            }
        }
    }
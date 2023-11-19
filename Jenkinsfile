pipeline {
    agent any
    tools {
        // Specify the correct Maven tool name
        maven 'MavenToolName'
    }
    stages {
        stage('Compile and Run Sonar Analysis') {
            steps {
                echo 'Running Maven clean verify sonar:sonar...'
                sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=kloud45 -Dsonar.organization=kloud45 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=8f444a51a34c57923785dfdbd7ef2aba4eb722b4'
            }
        }
        stage('Run SCA Analysis Using Synk') {
            steps {
                echo 'Running Snyk analysis...'
                withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                    sh 'mvn snyk:test -fn'
                }
            }
        }
        stage('Build') {
            steps {
                withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                    script {
                        echo 'Building Docker image...'
                        app = docker.build("asg")
                    }
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    echo 'Pushing Docker image...'
                    docker.withRegistry('736776108246.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials') {
                        app.push("latest")
                    }
                }
            }
        }
    }
}

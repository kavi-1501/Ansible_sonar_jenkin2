
pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    environment {
        SONAR_TOKEN = credentials('sonarqube-k') // Jenkins secret text credentials
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/kavi-1501/Ansible_sonar_jenkin2.git', branch: 'main'
            }
        }

        stage('Build WAR with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube-k') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=devops_git'
                }
            }
        }

        stage('Check Quality Gate') {
            steps {
                sh '''
                curl -s -u $SONAR_TOKEN: \
                "http://localhost:9000/api/qualitygates/project_status?projectKey=devops_git" \
                | grep -q '"status":"ERROR"' && echo "Quality Gate FAILED" && exit 1 || echo "Quality Gate PASSED"
                '''
            }
        }


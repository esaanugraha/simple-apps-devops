pipeline {
    agent { label 'docker-label' }
    
    tools {nodejs "nodes-simpleapps"}

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/esaanugraha/simple-apps-devops.git'
            }
        }
        stage('Build') {
            steps { 
                sh '''cd apps
                npm install'''
            }
        }
        stage('Testing') {
            steps {
                sh '''cd apps
                npm test
                npm run test:coverage'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''cd apps
                sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.14.22:9000 \
                -Dsonar.login=sqp_56cc0506274303c70c5bf9504b673aac245dc40e'''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker compose build
                docker compose up -d
                '''
            }
        }
        stage('Build and Push Images') {
            steps {
                sh '''
                cd apps
                docker build -t esanugraha/pipeline-simpleapps-project-apps .
                docker push esanugraha/pipeline-simpleapps-project-apps
                '''
            }
        }
    }
}
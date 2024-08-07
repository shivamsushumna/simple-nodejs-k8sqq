pipeline {
    agent any
    
    environment {
        // Define environment variables for Docker image names and GitLab registry URL
        FRONTEND_IMAGE = "shivamsingh733062/ema-demo/frontend"
        BACKEND_IMAGE = "shivamsingh733062/ema-demo/backend"
        MYSQL_IMAGE = "shivamsingh733062/ema-demo/mysql"
        GITLAB_REGISTRY_URL = "https://gitlab.com/shivamsingh733062/ema-demo.git"
        GITLAB_RE = "registry.gitlab.com"
        GITLAB_CREDENTIALS_ID = "gitlab-username"
        GITUSER = credentials('GITLAB_USER')
        GITPASS = credentials('GITLAB_PASSWORD')
        NEWUSEPASS = "newgitlabtoken"
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository
                git url: env.GITLAB_REGISTRY_URL, branch: "production"
            }
        }
        
        stage('Build and Push Frontend Image') {
            when {
                changeset "frontend/**"
            }
            steps {
                script {
                    // Build the frontend Docker image
                    def frontendImage = docker.build("${GITLAB_RE}/${FRONTEND_IMAGE}:${env.BUILD_NUMBER}", "./frontend")
                    
                    // Push the Docker image to GitLab Container Registry
                    docker.withRegistry("https://${GITLAB_RE}", "${GITLAB_CREDENTIALS_ID}") {
                        frontendImage.push
                    

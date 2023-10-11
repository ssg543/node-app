pipeline {
    agent any

    stages {
        stage('Checkout from Git') {
            steps {
                checkout scm
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh 'docker build -t myapp:latest .'
                }
            }
        }

        stage('Publish to JFrog') {
            steps {
                sh 'docker login -u JFROG_USERNAME -p JFROG_API_KEY JFROG_REGISTRY_URL'
                sh 'docker push myapp:latest'
            }
        }

        stage('Black Duck Scan') {
            steps {
                // Use the Black Duck CLI or integrate it with your CI/CD system
                sh 'blackduck scan'
            }
        }

        stage('Trufflehog Scan') {
            steps {
                sh 'trufflehog scan .'
            }
        }

        stage('SonarQube Analysis') {
            script {
              def scannerHome = tool 'SonarQube Scanner';
              withSonarQubeEnv('http://10.23.56.78:9000') {
                sh "${scannerHome}/bin/sonar-scanner"
            }
        }
    }
}
                withSonarQubeEnv('$CC') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Deploy to Dev') {
            steps {
                sh 'kubectl apply -f dev-deployment.yaml'
            }
        }

        stage('Deploy to QA') {
            steps {
                sh 'kubectl apply -f qa-deployment.yaml'
            }
        }

        stage('Deploy to Prod') {
            steps {
                sh 'kubectl apply -f prod-deployment.yaml'
            }
        }

        stage('Prometheus and Grafana') {
            steps {
                // Configure and deploy Prometheus and Grafana
                // This will depend on your specific setup
                // You might need to apply configuration files and deploy these tools to Kubernetes
            }
        }

        stage('Kubernetes Deployment') {
            steps {
                // Deploy your application to Kubernetes
                // Use kubectl or Kubernetes deployment scripts
                sh 'kubectl apply -f k8s-deployment.yaml'
            }
        }
    }
}

post {
    success {
        // Add post-build actions or notifications on success
        emailext subject: 'Build Successful',
        body: 'The Jenkins build was successful.',
        to: 'email@example.com'
    }
    failure {
        // Add post-build actions or notifications on failure
        emailext subject: 'Build Failed',
        body: 'The Jenkins build failed. Please check the build logs for details.',
        to: 'email@example.com'
    }
}

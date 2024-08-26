pipeline {
    
    agent any
    
    tools {
        git 'Default' // This should match the name you configured in Global Tool Configuration
    }

    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from a Git repository
                git branch: 'main', url: 'https://github.com/ramsjenu/data-engineering-pipeline.git'
            }
        }

        stage('Prepare Environment') {
            steps {
                // Assuming .env file is stored in a secure location
                sh 'cp /Users/vrams/Master/81-Github/DataEngineering/00-master/DEG08-data-engineering-ci-cd-pipieline/.env .'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    docker.image('python:3.8').inside('--network de-master') {
                        sh 'pip install -r scripts/requirements.txt'
                    }
                }
            }
        }
    
//        stage('Run Tests') {
//            steps {
//               script {
//                    docker.image('python:3.8').inside('--network de-master') {
//                        sh 'python -m unittest discover -s scripts'
//                    }
//               }
//            }
//        }

        stage('Run ETL') {
            steps {
                script {
                    docker.image('python:3.8').inside('--network de-master') {
                        sh 'pip install python-dotenv'
                        sh 'pip install psycopg2-binary'
                        sh 'pip install minio'
                        sh 'python scripts/etl.py'
                    }
                }
            }
        }
    }
    

//    post {
//        // Notifications or cleanup after the pipeline finishes
//        always {
//            archiveArtifacts artifacts: '**/*.csv', allowEmptyArchive: true
//        }
//    }
}


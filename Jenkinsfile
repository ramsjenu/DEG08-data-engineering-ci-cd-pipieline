pipeline {
    
    agent any
    
    environment {
        DATABASE_NAME = credentials('db-name-id')
        USER_NAME = credentials('user-name-id')
        PASSWORD_VALUE = credentials('password-id')
        HOST_NAME = credentials('host-name-id')
        PORT_NUMBER = credentials('port-number-id')
    }

    tools {
        git 'Default' // This should match the name you configured in Global Tool Configuration
    }

    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from a Git repository
                git branch: 'main', url: 'https://github.com/ramsjenu/DEG08-data-engineering-ci-cd-pipieline.git'
            }
        }

   #     stage('Prepare Environment') {
   #             // Assuming .env file is stored in a secure location
   #         steps {
   #             sh 'cp /home/.env .'
   #         }
   #     }

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


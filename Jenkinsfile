pipeline {
    agent any

    environment {
        function_name = 'samplejava'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Build'
                sh 'mvn package'
            }
        }
        stage("sonarqube analysis") {
              agent any
              steps {
                  withSonarQubeEnv('sonar') {
                      sh 'mvn clean package sonar:sonar'
                  }
              }
         }

        stage('Push') {
            steps {
                echo 'Push'

                sh "aws s3 cp target/sample-1.0.3.jar s3://javasample123"
            }
        }

        stage('Deploy') {
            steps {
                echo 'Build'

                sh "aws lambda update-function-code --function-name $function_name --region us-east-2 --s3-bucket javasample123 --s3-key sample-1.0.3.jar"
            }
        }
    }
}

pipeline{
    agent{
        label 'jenkins-inbound-agent'
    }
    stages{
        stage('build') {
            steps{
                echo "Building..."
                sh "./mvnw clean package -DskipTests"
                
            }
        }
        stage('test') {
            steps{
                echo "Testing..."
                sh "./mvnw test"
            }
        }
        stage('push artifact') {
            steps{
                echo "Testing"
            }
        }
    }
}
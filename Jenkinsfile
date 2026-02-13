pipeline{

    environment {
        // Extract version from pom.xml
        VERSION = sh(script: "./mvnw help:evaluate -Dexpression=project.version -q -DforceStdout", returnStdout: true).trim()
        ARTIFACT_ID = sh(script: "./mvnw help:evaluate -Dexpression=project.artifactId -q -DforceStdout", returnStdout: true).trim()
        GROUP_ID = sh(script: "./mvnw help:evaluate -Dexpression=project.groupId -q -DforceStdout", returnStdout: true).trim()
    }
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
            steps {
                nexusArtifactUploader(
                nexusVersion: 'nexus3',
                protocol: 'http',
                nexusUrl: '172.19.0.1:8081',
                groupId: "${GROUP_ID}",
                version: "${VERSION}",
                repository: 'maven-releases',
                credentialsId: 'nexus-jenkins-user',
                artifacts: [
                    [artifactId: "${ARTIFACT_ID}",
                    classifier: '',
                    file: "target/${ARTIFACT_ID}-${VERSION}.jar",
                    type: 'jar']
                ]
            )
            }
        }
        
    }
    post{
        success{
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}
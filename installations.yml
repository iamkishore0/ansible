pipeline {
    agent any
    tools {
        maven "Maven"
    }

    stages {
        stage('Clone_code') {
            steps {
                git 'https://github.com/wakaleo/game-of-life'
            }
        }
        stage('mvn_build') {
            steps {
              sh 'mvn clean install -DskipTests' 
            }
        }
        stage ('mvn_unit_test') {
            steps {
                 sh 'mvn test'
            }
        }
        stage ('mvn_integration_test') {
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }
        stage ('nexus_artifactory_upload') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: '$BUILD_TIMESTAMP', classifier: '', file: 'gameoflife-web/target/gameoflife.war', type: 'war']], credentialsId: 'c38b32c5-3a8a-46ad-bd6c-a1187731a6b4', groupId: 'kishore', nexusUrl: '54.90.205.237:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'gameoflife', version: '$BUILD_ID'
            }
        }
        stage ('deploy_to_staging_server_tomcat') {
            steps {
                deploy adapters: [tomcat8(credentialsId: 'e36ba4c9-e841-4b76-ab61-a3b07e9003ff', path: '', url: 'http://52.23.230.207:8080/')], contextPath: null, war: '**/*.war'
            }
        }
    }
}


pipeline {
    agent any
    tools {
        maven 'M3'
    }
    stages {
        stage('Git') {
            steps {
                git branch: 'main', url: 'https://github.com/chandu2266/live01.git'
            }
        }
        stage('build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('code qualirt analysis') {
            steps {
                withSonarQubeEnv(installationName: 'sonarqube-live01', credentialsId: 'live01') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('upload to nexus ') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'vamsi', classifier: '\'\'', file: 'target/live.war', type: 'war']], credentialsId: 'nexus', groupId: 'vamsi.maven.com', nexusUrl: '3.108.61.142:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'project-1', version: '0.0.1'
            }
        }
        stage('deploy') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://43.204.143.238:8080/')], contextPath: 'live01', war: '**/*.war'
            }
        }
    }
}

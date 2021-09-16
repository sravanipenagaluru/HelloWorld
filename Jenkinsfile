pipeline {
    agent any 
    tools {
        maven 'Maven'
    }

    stages {
        stage('gitcheckout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'git_credentials', url: 'https://github.com/sravanipenagaluru/HelloWorld.git']]])
            }
        }
        
        stage('build') {
            steps {
                bat 'mvn clean install'
            }
        }
        
        stage('codeQuality') {
            steps {
                bat 'mvn sonar:sonar'
            }
        }
        
        
        stage('nexus') {
            steps {
                bat 'mvn clean deploy -P release'
            }
        }
        
        stage('deploy') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'TomcatPasswd', path: '', url: 'http://localhost:4343/')], contextPath: 'HelloWorld', war: '**/*.war'
            }
        }
    }
}

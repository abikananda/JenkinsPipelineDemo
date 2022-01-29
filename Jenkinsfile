pipeline {
    agent any
    tools { 
        maven 'Maven-3.8.4'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/abikananda/HelloJenkins.git'
            }
        }
        stage('Static Code Analysis') {
            steps {
                sh 'mvn pmd:pmd'
                sh 'mvn checkstyle:checkstyle'
                sh 'mvn findbugs:findbugs'
            }
        }
        stage('Run Unit Test Cases') {
            steps {
                sh 'mvn clean test'
            }
        }
        stage('Publish Test Reports') {
            steps {
                junit '**/target/surefire-reports/TEST-*.xml'
            }
        }
        stage('Build Code') {
            steps {
                sh 'mvn package -DskipTests=true'
            }
        }
        stage('Archive Results') {
            steps {
                archiveArtifacts 'target/*.war'
            }
        }
        stage('Deploy Application') {
            steps {
                deploy adapters: [tomcat8(credentialsId: 'tomcat_deployer', path: '', url: 'http://34.215.104.249:8080/')], contextPath: 'HelloJenkinsPipeline', war: '**/target/*.war'
            }
        }
    }
}

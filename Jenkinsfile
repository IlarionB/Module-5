node('built-in') {
    stage('Clean and heckout code') {
        cleanWs()
        checkout([$class: 'GitSCM', branches: [[name: '*/dev-ans']], extensions: [], userRemoteConfigs: [[credentialsId: 'Jenkins-Github-Credentials', url: 'git@github.com:IlarionB/hello-world-war.git']]])
    }
    stage('SonarQube scan') {
        withSonarQubeEnv(installationName: 'sonarserver') {
            sh 'mvn clean sonar:sonar'
        }
    }
    stage('Build') {
        sh 'mvn compile'
    }
    stage('Test') {
        sh 'mvn test'
    }
    stage('Package') {
        sh 'mvn package'
    }
    stage('Docker build + tag') {
        sh 'git clone https://github.com/IlarionB/Infra.git'
        dir('/opt/tomcat/.jenkins/workspace/Module 5/Infra'){
            sh 'git checkout dev'
            sh 'cp Dockerfile /opt/tomcat/.jenkins/workspace/Module 5' 
        }
        sh 'cp /opt/tomcat/.jenkins/workspace/Module 5/target/hello-world-war-1.0.0.war /opt/tomcat/.jenkins/workspace/Module 5/hello-world-war-1.0.0.war'
        sh 'docker build -t hello-world-war:$BUILD_ID .'
    }
}

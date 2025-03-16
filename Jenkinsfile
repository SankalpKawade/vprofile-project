pipeline {
    agent any
    tools {
        jdk "JDK17"
        maven "MAVEN3.9"
    }
    environment {
        SNAP_REPO = 'project-snapshot'
		NEXUS_USER = 'admin'
		NEXUS_PASS = 'Admin1'
		RELEASE_REPO = 'project-release'
		CENTRAL_REPO = 'project-maven-central'
		NEXUSIP = '172.31.29.168'
		NEXUSPORT = '8081'
		NEXUS_GRP_REPO = 'project-group-repo'
        NEXUS_LOGIN = 'nexuslogin'
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
    }
    stages {
        stage('Build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "Now Archiving."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
    stage('UNIT TEST'){
            steps {
                sh 'mvn -s settings.xml test'
            }
    }
     stage ('CODE ANALYSIS WITH CHECKSTYLE'){
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result Hurrayy!!'
                }
            }
    }
    
        stage("UploadArtifact"){
            steps{
                nexusArtifactUploader(
                  nexusVersion: 'nexus3',
                  protocol: 'http',
                  nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
                  groupId: 'QA',
                  version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                  repository: "${RELEASE_REPO}",
                  credentialsId: "${NEXUS_LOGIN}",
                  artifacts: [
                    [artifactId: 'vproapp',
                     classifier: '',
                     file: 'target/vprofile-v2.war',
                     type: 'war']
                  ]
                )
            }
        }
}
}
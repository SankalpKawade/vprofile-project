pipeline {
    agent any
    tools {
        jdk "JDK17"
        maven "Maven3.9"
    }
    environment {
        SNAP_REPO = 'project-snapshot'
		NEXUS_USER = 'admin'
		NEXUS_PASS = 'Admin@1'
		RELEASE_REPO = 'project-release'
		CENTRAL_REPO = 'project-maven-central'
		NEXUSIP = '50.19.144.51'
		NEXUSPORT = '8081'
		NEXUS_GRP_REPO = 'project-group-repo'
        NEXUS_LOGIN = 'nexuslogin'
        SONARSERVER = 'SonarServer'
        SONARSCANNER = 'SonarScanner'
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
    }
    stage('UNIT TEST'){
            steps {
                sh 'mvn test'
            }
    }
     stage ('CODE ANALYSIS WITH CHECKSTYLE'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
    }

}
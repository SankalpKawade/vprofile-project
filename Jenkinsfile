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
     stage('Sonar Analysis') {
            environment {
                scannerHome = tool "${SONARSCANNER}"
            }
            steps {
               withSonarQubeEnv("${SONARSERVER}") {
                   sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
              }
            }
    }

}
}
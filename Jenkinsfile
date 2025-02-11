pipeline{
    agent any
    tools{
        jdk "jdk-17"
        maven "Maven3.9"
    }
    environment{
        SNAP_REPO = 'snapshot'
		NEXUS_USER = 'admin'
		NEXUS_PASS = 'Admin123'
		RELEASE_REPO = 'vprofile-release'
		CENTRAL_REPO = 'vpro-maven-central'
		NEXUSIP = '172.31.0.134'
		NEXUSPORT = '8081'
		NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
    }
    stages{
        stage('Build'){
            steps{
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post{
                success{
                    echo "Now archiving."
                    archiveArtificats artifacts: '**/*.war'
                }
            }
        }

        stage('Test'){
            steps{
                sh 'mvn test'
            }
        }

        stage(){
            sh 'mvn checkstyle:checkstyle'
        }
    }
}
pipeline {
	agent any	
/*	
	tools {
        maven "maven3"
        jdk "OracleJDK8"
    }
*/	
    environment {
        SNAP_REPO = 'snapshort'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        RELEASE_REPO = 'vprofile-release'
        CENTRAL_REPO = 'vpro-maven-central'
        NEXUSIP = '54.221.83.243'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'vprofile-maven-group'
        NEXUS_CREDENTIAL_ID = "nexuslogin"
        
    }
	
    stages{
        
        stage('BUILD'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
        }
    }
}

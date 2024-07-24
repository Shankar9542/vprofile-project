pipeline {
    agent any

    tools {
        maven "MAVEN3"
        jdk "OracleJDK11"
    }

    environment {
        SNAP_REPO = 'snapshort'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        RELEASE_REPO = 'vprofile-release'
        CENTRAL_REPO = 'vpro-maven-central'
        NEXUSIP = '3.87.169.236'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'vprofile-maven-group'
        NEXUS_CREDENTIAL_ID = 'nexuslogin'
    }
    
    stages {
        stage('BUILD') {
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

        stage('Test') {
            steps {
                sh 'mvn -s settings.xml test'
            }
        }

        stage('Checkstyle Analysis') {
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''
                        mvn sonar:sonar \
                            -Dsonar.projectKey=javalogin \
                            -Dsonar.host.url=http://3.88.220.182 \
                            -Dsonar.login=c14a679c0c3b8002d74499f177e666e2786e79a2
                    '''
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
                      credentialsId: "${NEXUS_CREDENTIAL_ID}", 
                      artifacts: [
                        [artifactId: 'vproapplication',
                         classifier: '',
                         file: 'target/vprofile-v2.war',
                         type: 'war']
                      ]
                    )
                }
            }    
    }
}

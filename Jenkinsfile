pipeline {
    agent any

    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }

    environment {
        SNAP_REPO = 'snapshort'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        RELEASE_REPO = 'vprofile-release'
        CENTRAL_REPO = 'vpro-maven-central'
        NEXUSIP = '52.90.233.177'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'vprofile-maven-group'
        NEXUS_CREDENTIAL_ID = "nexuslogin"
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
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
                    sh 'mvn sonar:sonar'
                }
            }
        }
        
        stage('Sonar Analysis') {
            steps {
                def scannerHome = tool 'sonarscanner'
                withSonarQubeEnv('sonarqube') {
                    sh "${scannerHome}/bin/sonar-scanner " 
                       "-Dsonar.login=admin " 
                       "-Dsonar.password=admin " 
                       "-Dsonar.projectKey=javalogin " 
                    mvn sonar:sonar \
                      -Dsonar.projectKey=javalogin \
                      -Dsonar.host.url=http://100.26.111.73 \
                      -Dsonar.login=a94673787b520bb47b5e2b7b134b50345f6a25df
                }
            }
        }
    }
}

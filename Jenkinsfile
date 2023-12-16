pipeline {
    agent { label 'MAVEN'}
    options {
        timeout(time: 1, unit: 'HOURS')
    }
    triggers {
        pollSCM('0 1 * * *')
    }
    stages {
        stage(git) {
            steps {
                git url: 'https://github.com/siddhaantkadu/openmrs-core.git',
                branch: 'sprint-release'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    archiveArtifacts artifacts: '**/openmrs.war'
                    junit testResults: '**/TEST-*.xml'
                }
                failure { 
                    mail subject: '$JOB_BASE_NAME Failed',
                         from: 'siddhant.hemant.kadu@vz.com',
                         to: 'vzi-devops@vz.com',
                         body: 'Refer Here for Build URL $BUILD_URL'
                }
            }
        }
        
    }
}
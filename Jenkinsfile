pipeline {
    agent { label 'UBUNTU_NODE2'}
    triggers { pollSCM('* * * * *')}
    stages {
        stage('vcs'){
            steps {
                git url: 'https://github.com/srikanthvelma-jenkins/spring-petclinic.git',
                    branch: 'develop'
            }
        }
        stage('build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('copy build') {
            steps{
                sh "mkdir -p /tmp/archive/${JOB_NAME}/${BUILD_ID} && cp ./target/spring-petclinic-*.jar /tmp/archive/${JOB_NAME}/${BUILD_ID}/"
                sh "aws s3 sync /tmp/archive/${JOB_NAME}/${BUILD_ID} s3://srikanthcicd"
            }
        }
        stage('postbuild') {
            steps {
                archiveArtifacts artifacts: '**/target/spring-petclinic-3.0.0-SNAPSHOT.jar'
                                 junit '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}
pipeline{
    agent any

    environment{
        MAVEN_CREDENTIAL = credentials('heartpattern-maven-repository')
    }

    stages{
        stage('daemon'){
            steps{
                sh './gradlew -PnexusUser=${MAVEN_CREDENTIAL_USR} -PnexusPassword=${MAVEN_CREDENTIAL_PSW} --daemon'
            }
        }
        stage('clean'){
            steps{
                sh './gradlew -PnexusUser=${MAVEN_CREDENTIAL_USR} -PnexusPassword=${MAVEN_CREDENTIAL_PSW} clean'
            }
        }
        stage('compile'){
            steps{
                sh './gradlew -PnexusUser=${MAVEN_CREDENTIAL_USR} -PnexusPassword=${MAVEN_CREDENTIAL_PSW} classes testClasses'
            }
        }
        stage('test'){
            steps{
                sh './gradlew -PnexusUser=${MAVEN_CREDENTIAL_USR} -PnexusPassword=${MAVEN_CREDENTIAL_PSW} test'
            }
        }
        stage('create distZip'){
            steps{
                sh './gradlew -PnexusUser=${MAVEN_CREDENTIAL_USR} -PnexusPassword=${MAVEN_CREDENTIAL_PSW} distZip distTar'
            }
        }
        stage('publish'){
            steps{
                sh './gradlew -PnexusUser=${MAVEN_CREDENTIAL_USR} -PnexusPassword=${MAVEN_CREDENTIAL_PSW} publish'
            }
        }
    }
    post{
        always{
            archiveArtifacts artifacts: 'build/distributions/*', fingerprint: true
            archiveArtifacts artifacts: 'build/libs/*', fingerprint: true
        }
    }
}
pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-id-secret')
        AWS_SECRET_ACCESS_KEY = credentials('aws-password-secret')
        AWS_S3_BUCKET = "gradffffffffffff"
        ARTIFACT_NAME = "sample-gradle.jar"
        AWS_EB_APP_NAME = "gradle"
        AWS_EB_APP_VERSION = "${BUILD_ID}"
        AWS_EB_ENVIRONMENT = "Gradle-env"
    }
    stages {
        stage('Validate') {
            steps {
                sh "chmod +x gradlew"
                sh "./gradlew clean"
            }
        }
        stage('Build') {
            steps {
                sh "./gradlew build"
                sh "./gradlew assemble"

            }
        }
        stage('Test') {
            steps {
                sh "./gradlew test"
            }
        }
        stage('Package') {
            steps {
                sh "./gradlew jar"
            }
            post{
                success{
                    archiveArtifacts artifacts: 'build/libs/samplegradle-0.0.1-SNAPSHOT.jar', followSymlinks: false
                }
            }
        }
        stage('Publish artifacts to S3 Bucket') {
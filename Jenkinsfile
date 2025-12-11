pipeline {
    agent none

    tools{
        maven 'mymaven'
    }

    parameters{
        string(name: 'Env', defaultValue: 'Test', description: 'Version to deploy')
        booleanParam(name: 'executeTests', defaultValue: true, description: 'Decide to run test cases')
        choice(name: 'APPVERSION', choices: ['1.1', '1.2', '1.3'], description: 'Select application version')

    }
    environment{
        BUILD_SERVER='ec2-user@172.31.8.244'
        IMAGE_NAME='devopstrainer/java-mvn-privaterepos:$BUILD_NUMBER'
        DEPLOY_SERVER='ec2-user@172.31.0.58'
    }

    stages {
        stage('Compile') {
            agent any
            steps {
                script{
                echo "Compiling the code in ${params.Env} environment"
                sh "mvn compile"
                }
            }
        }
        stage('CodeReview') {
            agent any
            steps {
                script{
                echo "Reviewing the code"
                sh "mvn pmd:pmd"
                }
            }
        }
        stage('UnitTest') {
            agent any
            when{
                expression { return params.executeTests == true }
            }
            steps {
                script{
                echo 'Testing the code'
                sh "mvn test"
                }
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Coverage Analysis') {
           // agent {label 'linux_slave'}
           agent any
            steps {
                script{
                echo 'Static Code Coverage Analysis of  the code'
                sh "mvn verify"
            }
        }
        }
        stage('Package') {
            agent any
            steps {
                script{
                echo 'Packaging the code  version ${params.APPVERSION} '
                sh "mvn package"
                    }
                }
            }
        
    
     stage('Publish the artifacts') {
            agent any
            steps {
                script{
                echo 'Publish the artifacts to JFrog'
                sh "mvn -U deploy -s settings.xml"
                    }
                }
            }
        }
    }



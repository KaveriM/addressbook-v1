pipeline {
    agent any

tools{
     maven 'mymaven'
}

    stages {
        stage('Compile') {
            steps {
                echo 'compiling the code'
                sh "mvn compile"
            }
        }
    
        stage('Code Review') {
            steps {
                echo 'Review code'
                sh "mvn pmd:pmd"
            }
        }
        stage('Unit test') {
            steps {
                echo 'unit test cases of the code'
                sh "mvn test"
            }
        }
        stage('Coverage Analysis') {
            steps {
                echo 'Static Coverage analysis of the code'
                sh "mvn verify"
            }
        }
        stage('Package') {
            steps {
                echo 'Package the code'
                sh "mvn package"
            }
        }
        stage('Publish the Artifacts') {
            steps {
                echo 'Publish the Artifacts to Jfrog'
                sh "mvn -U deploy -s settings.xml"
            }
        }
    }
}

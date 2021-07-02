pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                UiPathPack (
                    outputPath: '${WORKSPACE}\\Output', 
                    outputType: 'Process', 
                    projectJsonPath: "${WORKSPACE}", 
                    version: AutoVersion())
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                git 'https://gitlab.com/my-ci-test-group123/160_cicd_test_process_yaml.git'
                powershell 'C:\\Build\\Build.exe'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}

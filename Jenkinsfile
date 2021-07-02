pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                UiPathPack (
                    outputPath: "${WORKSPACE}\\Output", 
                    outputType: 'Process', 
                    projectJsonPath: "${WORKSPACE}", 
                    version: AutoVersion())
                
                echo 'Testing Dev..'
                
                echo 'Deploying Dev..'
                pwsh "C:\\Scripts\\Upload.exe targetOrchestratorName=dev targetOrchestratorURL=https://uipath.akoa.rocks/ targetTenantName=AKOA_GER_TEST targetUsername=RPADeployTest targetPassword=test1234 folderName=432_jenkins_RPA_test projectId=432 isRelease=true packagePath=C:\\JenkinsRoot\\jenkins_rpa_test\\master\\workspace\\Output"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                git 'https://gitlab.com/my-ci-test-group123/160_cicd_test_process_yaml.git'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}

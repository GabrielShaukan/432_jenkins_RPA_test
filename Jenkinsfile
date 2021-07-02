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
                
                echo 'Testing Integration..'
                
                
                
                dir("${WORKSPACE}") {
                    git url:'https://gitlab.com/my-ci-test-group123/160_cicd_test_process_yaml.git'
                }
                
                echo 'Deploying Integration..'
                powershell "C:\\Scripts\\Upload.exe targetOrchestratorName=dev targetOrchestratorURL=https://uipath.akoa.rocks/ targetTenantName=AKOA_GER_TEST targetUsername=RPADeployTest targetPassword=test1234 folderName=432_jenkins_RPA_test projectId=432 isRelease=true packagePath=C:\\JenkinsRoot\\jenkins_rpa_test\\master\\workspace\\Output"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing Konsolidation..'
                
                
                echo 'Deploying Integration..'
                powershell "C:\\Scripts\\Deploy.exe sourceOrchestratorName=dev sourceOrchestratorURL=https://uipath.akoa.rocks/ sourceTenantName=AKOA_GER_TEST sourceUsername=RPADeployTest sourcePassword=test1234 targetOrchestratorName=test targetOrchestratorURL=https://uipath.akoa.rocks/ targetTenantName=TestTenant3 targetUsername=RPADeployTest targetPassword=test1234 folderName=432_jenkins_RPA_test projectId=432 isRelease=true packagePath=C:\\JenkinsRoot\\jenkins_rpa_test\\master\\workspace\\Output"
            }
        }
        stage('Deploy') {
            steps {
                echo 'Testing Produktion..'
                
                echo 'Deploying Produktion..'
                powershell "C:\\Scripts\\Deploy.exe sourceOrchestratorName=dev sourceOrchestratorURL=https://uipath.akoa.rocks/ sourceTenantName=AKOA_GER_TEST sourceUsername=RPADeployTest sourcePassword=test1234 targetOrchestratorName=prod targetOrchestratorURL=https://uipath-test.akoa.rocks/ targetTenantName=Default targetUsername=RPADeployTest targetPassword=test1234 folderName=432_jenkins_RPA_test projectId=432 isRelease=true packagePath=C:\\JenkinsRoot\\jenkins_rpa_test\\master\\workspace\\Output"
                
                dir ("${WORKSPACE}\\Output") {
                    deleteDir()
                }
            }
        }
    }
}

pipeline {
    agent any
    environment {
        Username = credentials('OrchestratorUsername')
        Password = credentials('OrchestratorPassword')
        DevOrchestratorName = 'Integration'
        DevOrchestratorUrl = 'https://uipath.akoa.rocks/'
        DevTenantName = 'AKOA_GER_TEST'
        TestOrchestratorName = 'Konsolidation'
        TestOrchestratorUrl = 'https://uipath.akoa.rocks/'
        TestTenantName = 'TestTenant3'
        ProdOrchestratorName = 'Produktion'
        ProdOrchestratorUrl = 'https://uipath-test.akoa.rocks/'
        ProdTenantName = 'Default'
        FolderName = '432_jenkins_RPA_test'
        IsRelease = 'True'
        
    }
    
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
                
                echo 'Deploying Integration..'
                dir ("new") {
                    powershell "git clone https://gitlab.com/GabrielShaukan/foldernameexe.git"
                }
                
                powershell "${WORKSPACE}\\new\\foldernameexe\\Scripts\\Upload.exe targetOrchestratorName=$DevOrchestratorName targetOrchestratorURL=$DevOrchestratorUrl targetTenantName=$DevTenantName targetUsername=\$Username targetPassword=\$Password folderName=$FolderName isRelease=$IsRelease packagePath=${WORKSPACE}\\Output"
            }
        }
        stage('Test') {
            steps {
                input 'Deploy to test?'
                echo 'Testing Konsolidation..'
                
                
                echo 'Deploying Integration..'
                powershell "${WORKSPACE}\\new\\foldernameexe\\Scripts\\Deploy.exe sourceOrchestratorName=$DevOrchestratorName sourceOrchestratorURL=$DevOrchestratorUrl sourceTenantName=$DevTenantName sourceUsername=\$Username sourcePassword=\$Password targetOrchestratorName=$TestOrchestratorName targetOrchestratorURL=$TestOrchestratorUrl targetTenantName=$TestTenantName targetUsername=$Username targetPassword=$Password folderName=$FolderName isRelease=$IsRelease packagePath=${WORKSPACE}\\Output"
            }
        }
        stage('Deploy') {
            steps {
                input 'Deploy to test?'
                echo 'Testing Produktion..'
                
                echo 'Deploying Produktion..'
                powershell "${WORKSPACE}\\new\\foldernameexe\\Scripts\\Deploy.exe sourceOrchestratorName=$DevOrchestratorName sourceOrchestratorURL=$DevOrchestratorUrl sourceTenantName=$DevTenantName sourceUsername=\$Username sourcePassword=\$Password targetOrchestratorName=$ProdOrchestratorName targetOrchestratorURL=$ProdOrchestratorUrl targetTenantName=$ProdTenantName targetUsername=$Username targetPassword=$Password folderName=$FolderName isRelease=$IsRelease packagePath=${WORKSPACE}\\Output"
                
                dir ("${WORKSPACE}\\Output") {
                    deleteDir()
                }
                
                dir ("new") {
                    deleteDir()
                }
            }
        }
    }
}

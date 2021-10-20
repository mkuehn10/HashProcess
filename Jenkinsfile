pipeline {
    agent any

    environment {
        MAJOR = '1'
	    MINOR = '0'
	    UIPATH_ORCH_URL = "https://orchestrator.mk-dev.com/"
	    UIPATH_ORCH_TENANT_NAME = "Default"
	    UIPATH_ORCH_FOLDER_NAME = "Jenkins"
		UIPATH_TEST_SET_NAME = "HashProcess_TestSet"
    }

    stages {

        stage("Build Tests") {
            steps {
                echo "Building Tests with ${WORKSPACE}"
                UiPathPack (
                    outputPath: "Output\\Tests\\${env.BUILD_NUMBER}", 
                    outputType: 'Tests', 
                    projectJsonPath: "${WORKSPACE}", 
                    traceLevel: 'None', 
                    version: AutoVersion()
                )
            }
        }

        stage("Run Tests") {
            steps {
                UiPathTest (
                    credentials: UserPass('87c9735b-8a1c-4292-b0f8-d51f63199b35'), 
                    folderName: "${UIPATH_ORCH_FOLDER_NAME}", 
                    orchestratorAddress: "${UIPATH_ORCH_URL}", 
                    orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}", 
                    parametersFilePath: '', 
                    testResultsOutputPath: '', 
                    testTarget: TestProject(environments: '', testProjectPath: 'project.json'), 
                    traceLevel: 'None'
                )
                }
                }
    


        stage("Build Package") {
            steps {
                echo "Building Package with ${WORKSPACE}"
                dir ("Output") {
                    deleteDir()
                }
                UiPathPack (
                    outputPath: "Output\\${env.BUILD_NUMBER}", 
                    outputType: 'Process', 
                    projectJsonPath: "${WORKSPACE}", 
                    traceLevel: 'None', 
                    version: AutoVersion()
                )
            }
        }

        stage("Deploy Package") {
            steps {
                echo "Deploying Package with ${WORKSPACE}"
                UiPathDeploy (
                    credentials: UserPass('87c9735b-8a1c-4292-b0f8-d51f63199b35'), 
                    entryPointPaths: 'Main.xaml', 
                    environments: '', 
                    folderName: "${UIPATH_ORCH_FOLDER_NAME}", 
                    orchestratorAddress: "${UIPATH_ORCH_URL}", 
                    orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}", 
                    packagePath: "${WORKSPACE}\\Output", 
                    traceLevel: 'None'
                )
            }
        }

    }



}
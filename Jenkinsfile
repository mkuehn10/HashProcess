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
                    traceLevel: 'Verbose',
                    timeout: 10000
                )
            }
        }
    


        stage("Build Package") {
            steps {
                echo "Building Package with ${WORKSPACE}"
                UiPathPack (
                    outputPath: "${WORKSPACE}\\Output", 
                    outputType: 'Process', 
                    projectJsonPath: "${WORKSPACE}", 
                    traceLevel: 'Verbose', 
                    version: AutoVersion()
                )
            }
        }

        stage("Deploy Package") {
            steps {
                echo "Deploying Package with ${WORKSPACE}"
                bat(
                    "C:\\Program Files\\UiPath\\Studio\\UiPath.Studio.CommandLine.exe publish --project-path \"${WORKSPACE}\\project.json\" --target Custom --feed \"${WORKSPACE}\\project.json\""
                )
                UiPathDeploy (
                    credentials: UserPass('87c9735b-8a1c-4292-b0f8-d51f63199b35'), 
                    entryPointPaths: 'Main.xaml', 
                    environments: '', 
                    folderName: "${UIPATH_ORCH_FOLDER_NAME}", 
                    orchestratorAddress: "${UIPATH_ORCH_URL}", 
                    orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}", 
                    packagePath: "${WORKSPACE}\\Output", 
                    traceLevel: 'Verbose'
                )
            }
        }

    }



}
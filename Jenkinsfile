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
                    outputPath: "${WORKSPACE}\\Output\\Tests", 
                    outputType: 'Tests', 
                    projectJsonPath: "${WORKSPACE}", 
                    traceLevel: 'None', 
                    version: AutoVersion()
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
                    traceLevel: 'None', 
                    version: AutoVersion()
                )
            }
        }

    }



}
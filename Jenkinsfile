pipeline {
	    agent any
	

	        // Environment Variables
	        environment {
	        MAJOR = '1'
	        MINOR = '0'
	        //Orchestrator Services
	        UIPATH_ORCH_URL = "https://orchestrator.mk-dev.com/"
	        UIPATH_ORCH_TENANT_NAME = "Default"
	        UIPATH_ORCH_FOLDER_NAME = "Academy"
			UIPATH_TEST_SET_NAME = "HashProcess_TestSet"
	    }
	

	    stages {
	

	        // Printing Basic Information
	        stage('Preparing'){
	            steps {
	                echo "Jenkins Home ${env.JENKINS_HOME}"
	                echo "Jenkins URL ${env.JENKINS_URL}"
	                echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
	                echo "Jenkins JOB Name ${env.JOB_NAME}"
	                echo "GitHub BranhName ${env.BRANCH_NAME}"
	                checkout scm
	

	            }
	        }
	

	        stage('Build Tests') {
				steps {
					echo "Building package with ${WORKSPACE}"
					UiPathPack (
						outputPath: "${WORKSPACE}\\Output", 
						outputType: 'Tests', 
						projectJsonPath: "${WORKSPACE}", 
						traceLevel: 'None', 
						version: AutoVersion()
					)
				}
			}

			stage('Deploy Tests') {
				steps {
					echo "Deploying ${BRANCH_NAME} to Orchestrator"
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

			stage('Test') {
	            steps {
	                echo 'Testing..the workflow...'
					UiPathTest (
						credentials: UserPass('87c9735b-8a1c-4292-b0f8-d51f63199b35'), 
						folderName: "${UIPATH_ORCH_FOLDER_NAME}", 
						orchestratorAddress: "${UIPATH_ORCH_URL}", 
						orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}", 
						parametersFilePath: '', 
						testResultsOutputPath: 'result.xml', 
						testTarget: TestSet("${UIPATH_TEST_SET_NAME}"), 
						timeout: 10000, 
						traceLevel: 'None'
					)
	            }
	        }

			// stage('Deploy to UAT') {
	        //     steps {
	        //         echo "Deploying ${BRANCH_NAME} to UAT "
	        //         UiPathDeploy (
	        //         packagePath: "Output\\${env.BUILD_NUMBER}",
	        //         orchestratorAddress: "${UIPATH_ORCH_URL}",
	        //         orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
	        //         folderName: "${UIPATH_ORCH_FOLDER_NAME}",
	        //         environments: 'DEV',
	        //         credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: '3c43701f-a8d8-4fd9-a4d1-1ed40827bc2b'],
			// 		traceLevel: "None",
			// 		entryPointPaths: "Main.xaml"
	        //         //credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'APIUserKey'), 
	

	        // )
	        //     }
	        // }
	// Building Package
	        stage('Build Process') {
				when {
					expression {
						currentBuild.result == null || currentBuild.result == 'SUCCESS'
						}
				}
				steps {
					echo "Building package with ${WORKSPACE}"
					UiPathPack (
						  outputPath: "Output\\${env.BUILD_NUMBER}",
						  projectJsonPath: "project.json",
						  version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
						  useOrchestrator: false, 
						traceLevel: 'None'
						)
					}
	        }			
			
	         // Deploy to Production Step
	        stage('Deploy Process') {
				when {
					expression {
						currentBuild.result == null || currentBuild.result == 'SUCCESS' 
						}
				}
				steps {
	                echo "Deploying ${BRANCH_NAME} to UAT "
	                UiPathDeploy (
	                packagePath: "Output\\${env.BUILD_NUMBER}",
	                orchestratorAddress: "${UIPATH_ORCH_URL}",
	                orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
	                folderName: "${UIPATH_ORCH_FOLDER_NAME}",
	                environments: 'DEV',
	                credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: '3c43701f-a8d8-4fd9-a4d1-1ed40827bc2b'],
					traceLevel: "None",
					entryPointPaths: "Main.xaml"
					)
				}   
			}	
		
	    }

	


	    
	

	    // Options
	    options {
	        // Timeout for pipeline
	        timeout(time:80, unit:'MINUTES')
	        skipDefaultCheckout()
	    }
	

	

	    // 
	    post {
	        success {
	            echo 'Deployment has been completed!'
	        }
	        failure {
	          echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})"
	        }
	        always {
	            /* Clean workspace if success */
	            cleanWs()
	        }
	    }
	}
	    


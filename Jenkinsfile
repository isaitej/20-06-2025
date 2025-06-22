pipeline {
  agent any

  environment {
    PROJECT_FOLDER = 'Shared'
    PROJECT_PATH = 'C:\\Users\\Saiteja.Indarapu\\Documents\\UiPath\\UIPathTestingFolder\\20-06-2025'
    UIPCLI_PATH = 'C:\\Jenkins\\UiPathCI\\tools\\uipcli.exe'
    TOOLS_PLATFORM_PATH = 'C:\\Jenkins\\UiPathCI\\tools\\Platform\\net8.0-windows'
    OUTPUT_PATH = 'C:\\Jenkins\\UiPathCI\\output'

    ACCOUNT_FOR_APP = 'uipatntvskhu'
    APPLICATION_ID = '1234abcd-5678-efgh-9012-ijkl3456mnop'
    APPLICATION_SECRET = 'abcd1234secretvalue5678'
    ORCH_URL = 'https://cloud.uipath.com/uipatntvskhu/DefaultTenant'
    ORCH_TENANT = 'DefaultTenant'
    ORCH_FOLDER = 'Shared'
    TRACE_LEVEL = 'Verbose'
  }

  stages {
    stage('Confirm Jenkins User') {
      steps {
        powershell 'whoami'
      }
    }

    stage('Check NuGet Path') {
      steps {
        powershell 'Write-Host \"USERPROFILE is $env:USERPROFILE\"; dir "$env:USERPROFILE\.nuget\packages"'
      }
    }

    stage('Pack') {
      steps {
        powershell "\"${env:UIPCLI_PATH}\" package pack \"${env:PROJECT_PATH}\" -o \"${env:OUTPUT_PATH}\" --traceLevel ${env:TRACE_LEVEL} --libraryOrchestratorUrl ${env:ORCH_URL} --libraryOrchestratorTenant ${env:ORCH_TENANT} -A ${env:ACCOUNT_FOR_APP} -I ${env:APPLICATION_ID} -S ${env:APPLICATION_SECRET} --libraryOrchestratorApplicationScope \"OR.Folders OR.BackgroundTasks OR.TestSets OR.TestSetExecutions OR.TestSetSchedules OR.Settings.Read OR.Robots.Read OR.Machines.Read OR.Execution OR.Assets OR.Users.Read OR.Jobs OR.Monitoring\" --libraryOrchestratorFolder ${env:ORCH_FOLDER}"
      }
    }

    stage('Deploy') {
      steps {
        powershell "\"${env:UIPCLI_PATH}\" package push \"${env:OUTPUT_PATH}\\20-06-2025.1.0.0.nupkg\" --orchestratorUrl ${env:ORCH_URL} --orchestratorTenant ${env:ORCH_TENANT} -A ${env:ACCOUNT_FOR_APP} -I ${env:APPLICATION_ID} -S ${env:APPLICATION_SECRET} --orchestratorFolder ${env:ORCH_FOLDER}"
      }
    }

    stage('Run Job') {
      steps {
        powershell "\"${env:UIPCLI_PATH}\" run test --orchestratorUrl ${env:ORCH_URL} --orchestratorTenant ${env:ORCH_TENANT} -A ${env:ACCOUNT_FOR_APP} -I ${env:APPLICATION_ID} -S ${env:APPLICATION_SECRET} --orchestratorFolder ${env:ORCH_FOLDER} --testSetName \"YourTestSetName\""
      }
    }
  }

  post {
    failure {
      echo '❌ Build failed. Check logs for details.'
    }
  }
}
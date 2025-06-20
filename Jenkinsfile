pipeline {
  agent any

  environment {
    UIPCLI_PATH = 'C:\\Jenkins\\UiPathCI\\tools\\uipcli.exe'
    PROJECT_PATH = 'C:\\Users\\Saiteja.Indarapu\\Documents\\UiPath\\UIPathTestingFolder\\20-06-2025'
    OUTPUT_PATH = 'C:\\Jenkins\\UiPathCI\\output'
    PACKAGE_NAME = '20-06-2025'
    ORCH_URL = 'https://cloud.uipath.com/uipatntvskhu/DefaultTenant'
    TENANT = 'DefaultTenant'
    ACCOUNT_LOGICAL_NAME = 'uipatntvskhu'
    FOLDER_PATH = 'Shared'
  }

  stages {
    stage('Pack') {
      steps {
        powershell """
          & \$env:UIPCLI_PATH package pack `"\$env:PROJECT_PATH`" --output `"\$env:OUTPUT_PATH`"
        """
      }
    }

    stage('Deploy') {
      steps {
        withCredentials([
          string(credentialsId: 'uipath-client-id', variable: 'APP_ID'),
          string(credentialsId: 'uipath-client-secret', variable: 'APP_SECRET')
        ]) {
          powershell """
            & \$env:UIPCLI_PATH package deploy `
              `"\$env:OUTPUT_PATH\\\$env:PACKAGE_NAME.1.0.0.nupkg`" `
              --url `"\$env:ORCH_URL`" `
              --tenant `"\$env:TENANT`" `
              --accountLogicalName `"\$env:ACCOUNT_LOGICAL_NAME`" `
              --clientId `"\$env:APP_ID`" `
              --clientSecret `"\$env:APP_SECRET`"
          """
        }
      }
    }

    stage('Run Job') {
      steps {
        withCredentials([
          string(credentialsId: 'uipath-client-id', variable: 'APP_ID'),
          string(credentialsId: 'uipath-client-secret', variable: 'APP_SECRET')
        ]) {
          powershell """
            & \$env:UIPCLI_PATH job run `
              --url `"\$env:ORCH_URL`" `
              --tenant `"\$env:TENANT`" `
              --accountLogicalName `"\$env:ACCOUNT_LOGICAL_NAME`" `
              --clientId `"\$env:APP_ID`" `
              --clientSecret `"\$env:APP_SECRET`" `
              --folder `"\$env:FOLDER_PATH`" `
              --process `"\$env:PACKAGE

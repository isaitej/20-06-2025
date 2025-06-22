pipeline {
  agent any

  environment {
    UIPCLI_PATH = 'C:\\Jenkins\\UiPathCI\\tools\\uipcli.exe'
    PROJECT_PATH = 'C:\\Users\\Saiteja.Indarapu\\Documents\\UiPath\\UIPathTestingFolder\\20-06-2025\\project.json'
    PACKAGE_OUTPUT = 'C:\\Users\\Saiteja.Indarapu\\Documents\\UiPath\\Packages'
    PACKAGE_NAME = '20-06-2025'
    PACKAGE_VERSION = '1.0.0'
    ORCH_URL = 'https://cloud.uipath.com/uipatntvskhu/DefaultTenant'
    TENANT = 'DefaultTenant'
    ACCOUNT = 'uipatntvskhu'
    FOLDER = 'Shared'
    APPLICATION_ID = '9765da03-dd43-4915-8847-8fe03d64bfa8'
    APPLICATION_SECRET = 'YOUR-APP-SECRET'
    APP_SCOPE = 'OR.Folders OR.Execution OR.Jobs OR.Machines.Read OR.Robots.Read'
    PROCESS_NAME = '20-06-2025'
  }

  stages {

    stage('Confirm Jenkins User') {
      steps {
        powershell 'whoami'
      }
    }

    stage('Check NuGet Path') {
      steps {
        powershell 'Write-Host "USERPROFILE is $env:USERPROFILE"; Get-ChildItem "$env:USERPROFILE\\.nuget\\packages"'
      }
    }

    stage('Pack') {
      steps {
        powershell """
          & '${env:UIPCLI_PATH}' package pack "${env:PROJECT_PATH}" `
          --output "${env:PACKAGE_OUTPUT}" `
          --version "${env:PACKAGE_VERSION}" `
          --traceLevel Verbose
        """
      }
    }

    stage('Deploy') {
      steps {
        powershell """
          & '${env:UIPCLI_PATH}' package deploy `
          --package "${env:PACKAGE_OUTPUT}\\${env:PACKAGE_NAME}.${env:PACKAGE_VERSION}.nupkg" `
          --orchestratorUrl "${env:ORCH_URL}" `
          --tenant "${env:TENANT}" `
          --folder "${env:FOLDER}" `
          --accountForApp "${env:ACCOUNT}" `
          --applicationId "${env:APPLICATION_ID}" `
          --applicationSecret "${env:APPLICATION_SECRET}" `
          --applicationScope "${env:APP_SCOPE}" `
          --traceLevel Verbose
        """
      }
    }

    stage('Run Job') {
      steps {
        powershell """
          & '${env:UIPCLI_PATH}' job run `
          --orchestratorUrl "${env:ORCH_URL}" `
          --tenant "${env:TENANT}" `
          --folder "${env:FOLDER}" `
          --accountForApp "${env:ACCOUNT}" `
          --applicationId "${env:APPLICATION_ID}" `
          --applicationSecret "${env:APPLICATION_SECRET}" `
          --applicationScope "${env:APP_SCOPE}" `
          --process "${env:PROCESS_NAME}" `
          --traceLevel Verbose
        """
      }
    }

  }

  post {
    failure {
      echo "❌ Build failed. Check logs for details."
    }
    success {
      echo "✅ Build and deployment completed successfully!"
    }
  }
}

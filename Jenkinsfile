pipeline {
  agent any

  environment {
    PROJECT_NAME = '20-06-2025'
    PROJECT_PATH = 'C:\\Users\\Saiteja.Indarapu\\Documents\\UiPath\\UIPathTestingFolder\\20-06-2025'
    NUGET_OUTPUT_PATH = 'C:\\Jenkins\\UiPathCI\\Output'
    UIPCLI_PATH = 'C:\\Users\\Saiteja.Indarapu\\Documents\\UiPath\\UIPathTestingFolder\\20-06-2025\\tools\\uipcli.exe'

    ORCH_URL = 'https://cloud.uipath.com/uipatntvskhu/DefaultTenant'
    TENANT = 'DefaultTenant'
    ACCOUNT = 'uipatntvskhu'
    APP_ID = '9765da03-dd43-4915-8847-8fe03d64bfa8'
    APP_SECRET = '%Ab)AJCWq!f3%03v'
    FOLDER_PATH = 'Shared'
    TEST_SET = 'Testset'
  }

  stages {

    stage('Pack') {
      steps {
        echo "📦 Packing UiPath project..."
        powershell """
          & '${env:UIPCLI_PATH}' package pack '${env:PROJECT_PATH}' `
            -o '${env:NUGET_OUTPUT_PATH}' `
            --traceLevel Information
        """
      }
    }

    stage('Deploy') {
      steps {
        echo "🚀 Deploying to Orchestrator..."
        powershell """
          & '${env:UIPCLI_PATH}' package push '${env:NUGET_OUTPUT_PATH}\\${env:PROJECT_NAME}.1.0.0.nupkg' `
            --orchestratorUrl '${env:ORCH_URL}' `
            --orchestratorTenant '${env:TENANT}' `
            -A '${env:ACCOUNT}' `
            -I '${env:APP_ID}' `
            -S '${env:APP_SECRET}' `
            --libraryOrchestratorFolder '${env:FOLDER_PATH}'
        """
      }
    }

    stage('Run Job') {
      steps {
        echo "▶️ Running Test Set..."
        powershell """
          & '${env:UIPCLI_PATH}' testset run `
            --orchestratorUrl '${env:ORCH_URL}' `
            --orchestratorTenant '${env:TENANT}' `
            -A '${env:ACCOUNT}' `
            -I '${env:APP_ID}' `
            -S '${env:APP_SECRET}' `
            --testset '${env:TEST_SET}' `
            --folder '${env:FOLDER_PATH}'
        """
      }
    }
  }

  post {
    failure {
      echo "❌ Build failed. Please check logs above."
    }
    success {
      echo "✅ Build and test run completed successfully."
    }
  }
}
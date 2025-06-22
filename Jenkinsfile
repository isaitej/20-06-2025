pipeline {
  agent any

  environment {
    PROJECT_PATH = 'C:\\Users\\Saiteja.Indarapu\\Documents\\UiPath\\UIPathTestingFolder\\20-06-2025\\project.json'
    PACKAGE_OUTPUT = 'C:\\Jenkins\\UiPathPackages'
    ORCH_URL = 'https://cloud.uipath.com/uipatntvskhu/DefaultTenant'
    ORCH_TENANT = 'DefaultTenant'
    ORCH_FOLDER = 'Shared'
    ORCH_ACCOUNT = 'uipatntvskhu'
    APP_ID = '9765da03-dd43-4915-8847-8fe03d64bfa8'
    APP_SECRET = '%Ab)AJCWq!f3%03v'
    PROCESS_NAME = '20-06-2025'
    TEST_SET = 'Testset'
  }

  stages {

    stage('Pack') {
      steps {
        echo '📦 Packing UiPath project...'
        bat '"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe" -NoProfile -ExecutionPolicy Bypass -Command "uipcli package pack \\"%PROJECT_PATH%\\" --autoVersion -o \\"%PACKAGE_OUTPUT%\\" --outputType Process"'
      }
    }

    stage('Deploy') {
      steps {
        echo '🚀 Deploying package to Orchestrator...'
        bat '"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe" -NoProfile -ExecutionPolicy Bypass -Command "uipcli deploy process --project-path \\"%PROJECT_PATH%\\" --account-for-app \\"%ORCH_ACCOUNT%\\" --application-id \\"%APP_ID%\\" --application-secret \\"%APP_SECRET%\\" --orchestrator-url \\"%ORCH_URL%\\" --tenant \\"%ORCH_TENANT%\\" --folder \\"%ORCH_FOLDER%\\" --traceLevel Information"'
      }
    }

    stage('Run Job') {
      steps {
        echo '▶️ Triggering test set execution...'
        bat '"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe" -NoProfile -ExecutionPolicy Bypass -Command "uipcli testset run --testset \\"%TEST_SET%\\" --account-for-app \\"%ORCH_ACCOUNT%\\" --application-id \\"%APP_ID%\\" --application-secret \\"%APP_SECRET%\\" --orchestrator-url \\"%ORCH_URL%\\" --tenant \\"%ORCH_TENANT%\\" --folder \\"%ORCH_FOLDER%\\" --traceLevel Information"'
      }
    }

  }

  post {
    failure {
      echo '❌ Build failed. Please check logs above.'
    }
    success {
      echo '✅ Build and test run completed successfully.'
    }
  }
}
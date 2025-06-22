pipeline {
  agent any

  environment {
    // Set paths
    UIPATH_PROJECT_PATH = 'C:\\Users\\Saiteja.Indarapu\\Documents\\UiPath\\UIPathTestingFolder\\20-06-2025\\project.json'
    UIPATH_OUTPUT_PATH  = 'C:\\Jenkins\\UiPathCI\\Output'
    UIPATH_TOOL_PATH    = 'C:\\Jenkins\\UiPathCI\\tools\\uipcli.exe'
    
    // Fix PATH issue for Jenkins user
    PATH = "${env.PATH};C:\\Windows\\System32"
  }

  stages {

    stage('Pack') {
      steps {
        echo '📦 Packing UiPath project...'
        bat """
          "${UIPATH_TOOL_PATH}" package pack "%UIPATH_PROJECT_PATH%" -o "%UIPATH_OUTPUT_PATH%" --autoVersion
        """
      }
    }

    stage('Deploy') {
      steps {
        echo '🚀 Deployment stage will be added later.'
      }
    }

    stage('Run Job') {
      steps {
        echo '▶️ Run Job stage will be added later.'
      }
    }
  }

  post {
    failure {
      echo '❌ Build failed. Please check logs above.'
    }
    success {
      echo '✅ Build completed successfully.'
    }
  }
}
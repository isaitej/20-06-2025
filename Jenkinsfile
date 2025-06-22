pipeline {
  agent any

  environment {
    UIPATH_PROJECT_PATH = 'C:\\Users\\Saiteja.Indarapu\\Documents\\UiPath\\UIPathTestingFolder\\20-06-2025\\project.json'
    UIPATH_OUTPUT_PATH  = 'C:\\Jenkins\\UiPathCI\\Output'
    UIPATH_TOOL_PATH    = 'C:\\Jenkins\\UiPathCI\\tools\\uipcli.exe'
    DOTNET_ROOT         = 'C:\\Program Files\\dotnet'
    PATH = "${env.PATH};C:\\Windows\\System32;${DOTNET_ROOT}"
  }

  stages {
    stage('Pack') {
      steps {
        echo '📦 Packing UiPath project...'
        bat """
          echo Copying required .NET assemblies to output folder...
          copy "C:\\Program Files\\dotnet\\shared\\Microsoft.WindowsDesktop.App\\6.0.36\\System.Xaml.dll" "%UIPATH_OUTPUT_PATH%" > nul
          copy "C:\\Program Files\\dotnet\\shared\\Microsoft.WindowsDesktop.App\\6.0.36\\PresentationFramework.dll" "%UIPATH_OUTPUT_PATH%" > nul

          echo Starting UiPath pack...
          "${UIPATH_TOOL_PATH}" package pack "%UIPATH_PROJECT_PATH%" -o "%UIPATH_OUTPUT_PATH%" --autoVersion
        """
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
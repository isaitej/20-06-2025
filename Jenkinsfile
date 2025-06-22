pipeline {
  agent any

  environment {
    // Project and tool paths
    UIPATH_PROJECT_PATH = 'C:\\Users\\Saiteja.Indarapu\\Documents\\UiPath\\UIPathTestingFolder\\20-06-2025\\project.json'
    UIPATH_OUTPUT_PATH  = 'C:\\Jenkins\\UiPathCI\\Output'
    UIPATH_TOOL_PATH    = 'C:\\Jenkins\\UiPathCI\\tools\\uipcli.exe'

    // .NET and System paths
    DOTNET_PATH = 'C:\\Program Files\\dotnet'
    WIN_SYS32   = 'C:\\Windows\\System32'

    // Extend PATH so Jenkins finds all dependencies
    PATH = "${env.PATH};${DOTNET_PATH};${WIN_SYS32}"
  }

  stages {
    stage('Copy DLLs') {
      steps {
        echo '🔄 Copying required .NET assemblies...'
        bat """
          echo Copying System.Xaml.dll and PresentationFramework.dll...
          if exist "C:\\Program Files\\dotnet\\shared\\Microsoft.WindowsDesktop.App\\6.0.2\\System.Xaml.dll" copy "C:\\Program Files\\dotnet\\shared\\Microsoft.WindowsDesktop.App\\6.0.2\\System.Xaml.dll" "%UIPATH_OUTPUT_PATH%" > nul
          if exist "C:\\Program Files\\dotnet\\shared\\Microsoft.WindowsDesktop.App\\6.0.2\\PresentationFramework.dll" copy "C:\\Program Files\\dotnet\\shared\\Microsoft.WindowsDesktop.App\\6.0.2\\PresentationFramework.dll" "%UIPATH_OUTPUT_PATH%" > nul
        """
      }
    }

    stage('Pack') {
      steps {
        echo '📦 Packing UiPath project...'
        bat """
          "${UIPATH_TOOL_PATH}" package pack "%UIPATH_PROJECT_PATH%" -o "%UIPATH_OUTPUT_PATH%" --autoVersion
        """
      }
    }

    // You can uncomment these stages once the pack step is successful
    /*
    stage('Deploy') {
      steps {
        echo '🚀 Deployment stage placeholder.'
      }
    }

    stage('Run Job') {
      steps {
        echo '▶️ Run Job stage placeholder.'
      }
    }
    */
  }

  post {
    success {
      echo '✅ Build completed successfully.'
    }
    failure {
      echo '❌ Build failed. Please check logs above.'
    }
  }
}
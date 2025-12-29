pipeline {
    agent any
    
    stages {
        stage('Pull from Git') {
            steps {
                echo 'Pulling content from Git develop branch...'
                checkout scm
                
                // Windows batch commands
                bat '''
                    echo Creating directory...
                    if not exist "C:\\Jenkins_pulled_content" mkdir "C:\\Jenkins_pulled_content"
                    
                    echo Copying files...
                    xcopy /E /I /Y * "C:\\Jenkins_pulled_content\\"
                    
                    echo Git content successfully pulled to folder!
                    echo.
                    echo Files in destination:
                    dir "C:\\Jenkins_pulled_content"
                '''
            }
        }
    }
}

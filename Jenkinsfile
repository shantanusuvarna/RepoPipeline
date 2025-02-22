pipeline {
    agent any

    stages {

        stage('Clone RepoA') {
            steps {
                // Cloning RepoA which will generate warnings.log later
                git branch: 'master', url: 'https://github.com/shantanusuvarna/RepoA.git'
            }
        }

        stage('Modify Doxygen Config for Warnings') {
            steps {
                // Generate the default Doxyfile
                bat "doxygen -g Doxyfile"
                
                // Use PowerShell to update the Doxyfile (using Out-String to avoid issues)
                bat 'powershell -Command "(Get-Content Doxyfile | Out-String) -replace \'INPUT.*\', \'INPUT = src\' | Set-Content Doxyfile"'
                bat 'powershell -Command "(Get-Content Doxyfile | Out-String) -replace \'GENERATE_HTML.*\', \'GENERATE_HTML = YES\' | Set-Content Doxyfile"'
                bat 'powershell -Command "(Get-Content Doxyfile | Out-String) -replace \'WARN_LOGFILE.*\', \'WARN_LOGFILE = warnings.log\' | Set-Content Doxyfile"'
                
                // Print the Doxyfile for debugging
                bat "type Doxyfile"
            }
        }

        stage('Run Doxygen') {
            steps {
                // Run Doxygen to generate documentation and warnings.log
                bat "doxygen Doxyfile"
                // List files to confirm warnings.log is generated
                bat "dir"
            }
        }

        stage('Clone RepoC') {
            steps {
                // Clone RepoC into a subdirectory called "DoxygenLogParser"
                dir('DoxygenLogParser') {
                    git branch: 'main', url: 'https://github.com/shantanusuvarna/DoxygenLogParser'
                    // List files to verify that log_parser.py exists
                    bat "dir"
                }
            }
        }

        stage('Debug Workspace Paths') {
            steps {
                // Print the workspace path and search for warnings.log
                bat 'echo Workspace: %WORKSPACE%'
                bat 'dir /s /b warnings.log'
            }
        }

        stage('Run Log Parser') {
            steps {
                // Change into the RepoC folder where log_parser.py is located
                dir('DoxygenLogParser') {
                    // Debug: Show current directory and list files
                    bat 'echo Current directory: %CD%'
                    bat 'dir'
                    
                    // Run the Python log parser script using the full Python path.
                    // Replace the following path with the one from "where python"
                    bat '"C:\\Users\\SHANTANU\\AppData\\Local\\Programs\\Python\\Python313\\python.exe" logparser.py "%WORKSPACE%\\warnings.log" parsed_warnings.csv'
                    
                    // Debug: List files after running the script
                    bat 'dir'
                }
                // Archive the resulting CSV file
                archiveArtifacts artifacts: 'DoxygenLogParser/parsed_warnings.csv', fingerprint: true
            }
        }
    }
}
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Getting source code from GitHub'
                checkout scm
            }
        }

        stage('Verify Files') {
            steps {
                echo 'Checking project files'

                bat '''
                if not exist index.html exit /b 1
                if not exist style.css exit /b 1
                if not exist README.md exit /b 1
                '''

                echo 'All required files are present'
            }
        }

        stage('Run Tests') {
            steps {

                echo 'Running website test cases'

                // TC01: HTML file is not empty
                bat '''
                for %%A in (index.html) do if %%~zA EQU 0 exit /b 1
                '''

                // TC02: CSS file is not empty
                bat '''
                for %%A in (style.css) do if %%~zA EQU 0 exit /b 1
                '''

                // TC03: HTML references CSS
                bat '''
                findstr /I /C:"style.css" index.html >nul
                if errorlevel 1 exit /b 1
                '''

                // TC04: HTML contains HTML tag
                bat '''
                findstr /I /C:"<html" index.html >nul
                if errorlevel 1 exit /b 1
                '''

                echo 'All test cases passed!'
            }
        }

        stage('Deploy to GitHub Pages') {
            steps {

                withCredentials([
                    string(
                        credentialsId: 'github-pat',
                        variable: 'GIT_TOKEN'
                    )
                ]) {

                    bat '''
                    git push https://Harishkumar71204:%GIT_TOKEN%@github.com/Harishkumar71204/CI-CD-Assignment-Practice.git HEAD:refs/heads/gh-pages --force
                    '''

                }
            }
        }
    }

    post {
        success {
            echo 'Website deployed successfully!'
        }

        failure {
            echo 'Pipeline failed! Check Console Output.'
        }
    }
}
 i dont need to change anything here rihgt
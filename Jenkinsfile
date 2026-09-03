pipeline{
    agent any
    stages{
        stage('Checkout Code'){
            steps{
                git branch: 'main',
                url: 'https://github.com/Abishek-151/RESTFULBOOKER-PERFORMANCE-TEST.git'
            }
        }
        stage('Run Jmeter Test'){
            steps{
                bat '''
                if exist "result.jtl" del /f /q "result.jtl"
                if exist "html-report" rmdir /s /q "html-report"
                "%JMETER_HOME%\\bin\\jmeter.bat" -n -t "RestfulBooker TestPlan.jmx" -l "result.jtl" -e -o "html-report"
                '''
            }
        }
        stage('Publish HTML Report'){
            steps{
                publishHTML([
                    reportDir: 'html-report',
                    reportFiles: 'index.html',
                    reportName: 'JMeter Performance Report',
                    keepAll: true,
                    alwaysLinkToLastBuild: true,
                    allowMissing: false
                    ])
            }
        }
        stage('Send Email'){
            steps{
                emailext(
                    subject: "JMeter Test - ${currentBuild.currentResult}",
                    body: """JMeter Performance Test Completed.
                    Build: ${env.BUILD_NUMBER}
                    Status: ${currentBuild.currentResult}
                    JMeter HTML report is available in jenkins.
                    """,
                    to: 'abishekn.kiaq@gmail.com'
                    )
            }
        }
    }
    }

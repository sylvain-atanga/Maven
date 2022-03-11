pipeline
{
    agent any
    stages
    {
        stage('ContinuousDownload') 
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/sylvain-atanga/Maven.git'
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins is unable to download the code from github', cc: '', from: '', replyTo: '', subject: 'Download failed', to: 'git.team@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinuousBuild') 
        {
            steps
            {
                script
                {
                    try
                    {
                        sh 'mvn package'
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins is unable to build artifact from code', cc: '', from: '', replyTo: '', subject: 'Build failed', to: 'development.team@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinuousDeployment') 
        {
            steps
            {
                script
                {
                    try
                    {
                        deploy adapters: [tomcat9(credentialsId: '5e216110-9656-45c8-bc75-bec87ddd167d', path: '', url: 'http://172.31.84.142:8080')], contextPath: 'testapp1', war: '**/*.war'
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins is unable to deploy artifact to Tomcat QA server', cc: '', from: '', replyTo: '', subject: 'Deploment failed', to: 'middleware.team@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinuousTesting') 
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/selenium-saikrishna/FunctionalTesting.git'
                        sh 'java -jar /var/lib/jenkins/workspace/DeclarativePipelineEmailEveryStep/testing.jar'
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Selenium test scripts are failing', cc: '', from: '', replyTo: '', subject: 'Testing failed', to: 'testing.team@gmail.com'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinuousDelivery') 
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/selenium-saikrishna/FunctionalTesting.git'
                        sh 'java -jar /var/lib/jenkins/workspace/DeclarativePipelineEmailEveryStep/testing.jar'
                    }
                    catch(Exception e1)
                    {
                        deploy adapters: [tomcat9(credentialsId: '5e216110-9656-45c8-bc75-bec87ddd167d', path: '', url: 'http://172.31.94.71:8080')], contextPath: 'prodapp1', war: '**/*.war'
                        exit(1)
                    }
                }
            }
        }
    }
}

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
                        mail bcc: '', body: 'Jenkins failed to download code from github repository', cc: '', from: '', replyTo: '', subject: 'Download failed', to: 'git.team@hbc.org'
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
                        mail bcc: '', body: 'Jenkins failed to build artifact from code', cc: '', from: '', replyTo: '', subject: 'Build failed', to: 'dev.team@hbc.org'
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
                        deploy adapters: [tomcat9(credentialsId: 'c0646f3e-9314-475b-9661-e31a0d23a6ca', path: '', url: 'http://172.31.86.88:8080')], contextPath: 'testapp', war: '**/*.war'   
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins failed to deploy artifact to QA environment', cc: '', from: '', replyTo: '', subject: 'Build failed', to: 'midleware.team@hbc.org'
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
                        git 'https://github.com/sylvain-atanga/FunctionalTesting.git'
                        sh 'java -jar /var/lib/jenkins/workspace/DeclarativePipeline/testing.jar'
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins shows fail after executing selenium scripts', cc: '', from: '', replyTo: '', subject: 'testing failed', to: 'testing.team@hbc.org'
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
                        deploy adapters: [tomcat9(credentialsId: 'c0646f3e-9314-475b-9661-e31a0d23a6ca', path: '', url: 'http://172.31.90.145:8080')], contextPath: 'prodapp', war: '**/*.war'
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins fail to deliver app to production environment', cc: '', from: '', replyTo: '', subject: 'delivery failed', to: 'delivery.team@hbc.org'
                        exit(1)
                    }
                }
            }
        }
    }
}

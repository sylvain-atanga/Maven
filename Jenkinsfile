pipeline
{
    agent any
    
    stages
    {
        stage('Continuous Download_Master')
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
                        mail bcc: '', body: 'Jenkins failed to download from SC repo', cc: '', from: '', replyTo: '', subject: 'download failed', to: 'development-team@hbc.org'
                        exit(1)
                    }
                }
            }
        }
        stage('Continuous Build_Master')
        {
            steps
            {
                script
                {
                    try
                    {
                        sh 'mvn package'
                    }
                    catch(Exception e2)
                    {
                        mail bcc: '', body: 'Jenkins failed to build artifact from code', cc: '', from: '', replyTo: '', subject: 'Build failed', to: 'dev=team @hbc.org'
                        exit(1)
                    }
                }
            }
        }
         stage('Continuous Deployment_Master')
         {
             steps
             {
                script
                {
                    try
                    {
                        deploy adapters: [tomcat9(credentialsId: 'cfadbe0d-44a4-46f4-9cbe-a5af5c3446c4', path: '', url: 'http://172.31.27.194:8080')], contextPath: 'test-app', war: '**/*war'    
                    }
                    catch(Exception e3)
                    {
                        mail bcc: '', body: 'Jenkins failed to deploy to QA', cc: '', from: '', replyTo: '', subject: 'Deploment failed ', to: 'midleware-team@hbc.org'
                        exit(1)
                    }
                }
             }
         }
        stage('Continuous Testing_Master')
        {
            steps
            {
                script
                {
                    try
                    {
                        git 'https://github.com/sylvain-atanga/FunctionalTesting.git'
                        sh 'java -jar /var/lib/jenkins/workspace/MB-PL_master/testing.jar'
                    }
                   catch(Exception e4)
                    {
                        mail bcc: '', body: 'Jenkins testing using selenium script fail', cc: '', from: '', replyTo: '', subject: 'Testing failed ', to: 'testinge-team@hbc.org'
                        exit(1)
                    }
                }
            }
        }
        stage('Continuous Delivery_Master')
        {
            steps
            {
                script
                {
                    try
                    {
                        deploy adapters: [tomcat9(credentialsId: 'cfadbe0d-44a4-46f4-9cbe-a5af5c3446c4', path: '', url: 'http://172.31.26.111:8080')], contextPath: 'BK-app', war: '**/*war'
                    }
                    catch(Exception e5)
                    {
                        mail bcc: '', body: 'Jenkins failed to deliver app to production', cc: '', from: '', replyTo: '', subject: 'Delivery failed ', to: 'midleware-team@hbc.org'
                        exit(1)
                    }
                }
            }
        }
    }
}

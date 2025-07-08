pipeline{
  agent any
  stages{
    stage('checkout'){
      steps{
        git branch: 'main', url: 'https://github.com/dawrinmiller/test-django'
      }
    }
    stage('Login to ECR'){
      steps{
        withAWS(region: 'us-east-2', credentials: 'aws-creds'){
          powershell '''
          $password = aws acr get-login-password --region us-east-2
          docker login --username AWS --password $password 980048099386.dkr.ecr.us-east-2.amazonaws.com/test
          '''
        }
      }
    }
    stage('Build docker image'){
      steps{
        powershell '''
        docker build -t test:django
        docker tag test:django 980048099386.dkr.ecr.us-east-2.amazonaws.com/test:django
        '''
      }
    }
    stage('Pushing image to ECR'){
      steps{
        powershell '''
        docker push 980048099386.dkr.ecr.us-east-2.amazonaws.com/test:django
        '''
      }
    }
  }

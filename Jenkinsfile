pipeline{
    agent any
    environment{
        AWS_ACCESS_KEY_ID     = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')

        AWS_REGION = 'us-east-1'

        ECR_REPO_NAME="repodocker"

        ACCOUNT_ID='637423639805'



    }
    stages{
     stage('Checkout stage from GIT'){
           steps{
              script{
               git branch:'master',url:'https://github.com/Suba-Nandhini/docker_HelloWorld.git'

               }
           }
     }
     stage('JAR Creation'){
       steps{
              script{
                              bat "mvn clean"
                              echo ".......Cleaning the previous build......"
                              bat "mvn validate"
                              bat "mvn compile"
                              echo ".......Compiling the source code......"
                              bat "mvn package"
                              echo ".......Creating Jar files........"
                              bat "mvn verify"
                              bat "mvn install"
              }

       }

     }
     stage('Image creation in ECR'){
           steps{
            script{
                 def repoName = "${env.ECR_REPO_NAME}"
                 def awsRegion = "${env.AWS_REGION}"
                 def ecrUrl = "${env.ACCOUNT_ID}.dkr.ecr.${awsRegion}.amazonaws.com"


                     bat """
                           set REPO_NAME=${repoName}
                           set REGION=${awsRegion}
                           set ECR_URL=${ecrUrl}

                           aws ecr get-login-password --region %REGION% | docker login --username AWS --password-stdin  %ECR_URL%
                           docker build -t  %REPO_NAME% .
                           docker tag %REPO_NAME%:latest %ECR_URL%/%REPO_NAME%:latest
                           docker push  %ECR_URL%/%REPO_NAME%:latest
                     """
            }
           }



     }

    }
}



            
//             steps {
//              echo NODE_ENV
//              withCredentials([string(credentialsId: 'e8f8ff88-49e0-433a-928d-36a518cd30d6', variable: 'secver')]) {
//                 //  some block
//                 echo secver
//             }
//                          sh 'npm install'
//             }
            
//         }
        
//          stage('saveArtifact') {
//             steps {
//               archiveArtifacts artifacts: '**', followSymlinks: false
//             }
            
//         }
        
        
        
//     }
// }
pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID="553346259689"
        AWS_DEFAULT_REGION="eu-west-1" 
        IMAGE_REPO_NAME="testj"
        IMAGE_TAG="latest"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
   
    stages {
        
         stage('Logging into AWS ECR') {
            steps {
                script {
                sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
                }
                 
            }
        }
        
        stage('Cloning Git') {
            steps {
             checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/balchawar/aws_codebuild_codedeploy_nodeJs_demo.git']]])  
             checkout([$class: 'GitSCM', branches: [[name: '*/dev-mek']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/balchawar/aws_codebuild_codedeploy_nodeJs_demo.git']]])   
            }
        }
  
    // Building Dockerimages
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
        }
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"
                mail bcc: '', body: 'test email of jenkins pipeline!!', cc: 'yeabsira@eaglelionsystems.com', from: '', replyTo: '', subject: 'This is from Jenkins', to: 'melkamu@eaglelionsystems.com'
         }
        }
      }
    }
}


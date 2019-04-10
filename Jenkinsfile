pipeline{
  agent any
  stages{
      stage('build project')
      {
        steps
        {
          sh "mvn clean install"
        }
      }
      stage('Docker build')
        {
            steps
            {
                script
                {
                    // Build the docker image using a Dockerfile                                     
                    sh "sed 's!will!appcom!g' pom.xml" 
                    //sh "login="(aws ecr get-login --no-include-email --region us-east-2)" | sed 's/https:\/\// /'"
                    //sh "eval $login"
                    //sh "eval ${(aws ecr get-login --no include-email --region us-east-2)}"
                    //sh("eval \$(aws ecr get-login --no-include-email --region us-east-2 | sed 's|https://||') > demo.sh")
                    //sh" chmod 777 ./demo.sh"
                    //sh" ./demo.sh"                    
                    
                    //sh "docker tag demo:latest 630578467060.dkr.ecr.us-east-2.amazonaws.com/demo:$BUILD_NUMBER"
                    //sh "docker push 630578467060.dkr.ecr.us-east-2.amazonaws.com/demo:$BUILD_NUMBER"
                    docker.withRegistry("https://630578467060.dkr.ecr.us-east-2.amazonaws.com/demo", "ecr:us-east-2:4e98734b-6e1b-4025-9b36-9886838b99ce") {
                       //docker.build("demo:$BUILD_NUMBER",".")
                       def customImage = docker.build("demo:$BUILD_NUMBER")
                       //sh "docker tag demo:latest 630578467060.dkr.ecr.us-east-2.amazonaws.com/demo:$BUILD_NUMBER"
                       //docker.image("demo").push()
                       customImage.push()
                    }
                    
                } 
            }
            
        }
        stage('Deploy Ecr')
        {
           environment {
              
              REGION="us-east-2"
              REPOSITORY_NAME="demo"
              CLUSTER="samplecluster"
              FAMILY="sampletaskdefinition"
              NAME="sampletaskdefinition"
              SERVICE_NAME="sampleservice"
           }
           steps
           {
             script
             {               
                def REPOSITORY_URI=sh (script:"aws ecr describe-repositories --repository-names ${REPOSITORY_NAME} --region ${REGION} | jq .repositories[].repositoryUri")
                //sh "sed -e "s;%BUILD_NUMBER%;${BUILD_NUMBER};g" -e "s;%REPOSITORY_URI%;${REPOSITORY_URI};g" taskdef.json > ${NAME}-v_${BUILD_NUMBER}.json"
             } 
           }
        }     
  }
 }

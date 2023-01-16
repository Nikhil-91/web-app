pipeine{
    agent any
    
    tools{
        jdk "JAVA_HOME"
        jdk "JAVA_11"
        maven "MAVEN_HOME"
    }
    environment{
      registry=nikhildevops38/webapp   
    }
    
     stages{    
        stage('UNIT TEST'){
            steps {
                withEnv(["JAVA_HOME=${tool 'JAVA_HOME'}"]) {
                  // build steps go here
                  sh 'mvn test'
                }
                
            }
        }

       stage('INTEGRATION TEST'){
            steps {
                withEnv(["JAVA_HOME=${tool 'JAVA_HOME'}"]) {
                  // build steps go here
                  sh 'mvn verify -DskipUnitTests'
                }
                
            }
        }
        
       stage('SonarQube analysis') {
        steps{
                script{
            withSonarQubeEnv('sonarserver') {
               sh 'mvn sonar:sonar'
             } 
         }
        }
            }
       
        stage("Quality Gate"){
          steps{
                     script{
            timeout(time: 1, unit: 'HOURS') {    
            def qg= waitForQualityGate()    
            if (qg.status != 'OK') {       
                error "Pipeline aborted due to quality gate failure: ${qg.status}"     
            }       
          }
           }
          }


        }
     
         stage('Build docker')
        {
           steps{
             script{
              sh 'docker build . registry+:BUILD_NUMBER'
              withCredentials([string(credentialsId: 'dockerpasswd', variable: 'dockerpwd')]) {
                 docker login -u nikhildevops38 -p dockerpwd
                 docker push $registry:$BUILD_NUMBER
                    
                       }
                   }
           }
         }
           
        }
        
     }

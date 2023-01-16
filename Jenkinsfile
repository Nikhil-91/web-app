pipeline{
    agent any
    
    tools{
        jdk "JAVA_HOME"
        jdk "JAVA_11"
        maven "MAVEN_HOME"
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
               sh 'mvn clean package sonar:sonar'
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

        }
    	
     }

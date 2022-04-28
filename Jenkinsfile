pipeline {
   agent any
   
   tools {
     
      maven "maven"
   }
   
    parameters {
        string(name: 'host', description: 'API Manager Host Name', defaultValue: 'forrester.demo.axway.com')
        string(name: 'stage', description: 'Targer Environment to Deploy', defaultValue: 'staging')
        string(name: 'returnCodeMapping', description: 'Swagger Promote CLI return code mapping', defaultValue: '10:0')
        string(name: 'appName', description: 'API Builder Appname')
       
    }

   stages {
      stage('Import API to Axway API Manager') {
         steps {
            script{
               def configFile = 'config.json'
               def props = readJSON file: configFile
               if(stage.equals("preprod")){
                  def list = new ArrayList()
                  list.add("prod")
                  def tags = props.get("tags")
                  tags.accumulate("nextStage", list)
                  writeJSON file: configFile, json: props
               }
            }
            
            withCredentials([usernamePassword(credentialsId: "${stage}", usernameVariable: 'username', passwordVariable: 'password')])  {
               sh 'mvn clean exec:java -Dexec.args="-h ${host} -u ${username} -p ${password} -c  $configFile -force  -returnCodeMapping ${returnCodeMapping}"'
            }
     
         }
      }
   }
}

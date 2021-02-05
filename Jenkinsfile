node 
{ 
 stage ('SCM Checkout') 
{ 
 git 'https://github.com/ajit2525/JavaSpringMvcBlog-1.git' 
} 
 stage ('Test Package') 
{ 
 sh 'mvn test' 
} 
 stage ('Sonarqube ') 
{ 
  withSonarQubeEnv('Sonarqube') { 
   sh "mvn verify sonar:sonar" 
  } 
 }  
 try 
 { 
  stage ('Build Package') 
 { 
   sh 'mvn package' 
  }
 } 
 catch(err) 
  { 
   stage ('Email Notification') 
     { 
        emailext body: '''Hi 
        Your build has failed. Please rectify the error. 
        Regards''', subject: 'Build Failed', to: 'ajiteee2394@gmail.com' 
   } 
 }
 stage ('Artifact upload') 
{ 
  def server = Artifactory.server 'Artifactory' 
  def uploadSpec = """{ 
  "files": [ 
    { 
      "pattern": "/var/lib/jenkins/workspace/PluralSight1/target/*.war",
      "target": "Blog-snapshot" 
 } 
  ] 
    }""" 
  server.upload(uploadSpec)\ 
  } 
 }

stages {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git '{{module.git.url}}'
//    git credentialsId: 'xxx', url: '{{module.git.url}}'

      // Get the Maven tool.
      // ** NOTE: This  Maven tool must be configured
      // **       in the global configuration.
      mvnHome = tool 'maven'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean install -DskipTests -Ptest"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean install -DskipTests -Ptest/)
      }

      sh '/home/sunzone/data/jenkins_script/multi_build.sh 10.104.38.16:5000 test.rancher.manager back-end'
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }

// https://github.com/aatarasoff/spring-boot-example-for-jenkins-pipeline/blob/master/Jenkinsfile
//   def uploadJarToNexus(version) {
//     nexusArtifactUploader artifacts: [
//        [artifactId: 'demo',  file: "build/libs/demo-${version}.jar",  type: 'jar']
//      ], credentialsId: 'god', groupId: 'com.example', nexusUrl: 'nexus:8081',  nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-releases',  version: "${version}"
//   }   
}

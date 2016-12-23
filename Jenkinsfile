
node {
    
   // Mark the code checkout 'stage'....
   stage 'Checkout'
   sh('echo $M2_HOME')
   sh('echo $M2_HOME > ECHO')
   def stdout = readFile('ECHO').trim()
   print "out: " + stdout
   
   sh "export M2_HOME=/Users/jgaspard/Documents/DevTools/apache-maven-3.3.9"
   sh('echo $M2_HOME > ECHO')
   def stdout2 = readFile('ECHO').trim()
   print "out: " + stdout2

   // Get some code from a GitHub repository
   //non git url: 'git@github.com:financeactive/' + repository + '.git'
   //ok git url: 'https://github.com/jgaspard-fa/' + repo + '.git'
   //non git url: 'https://github.com/financeactive/' + repository
   git branch: 'develop', url: 'https://github.com/jgaspard-fa/gitflow-fa.git'
   
   // Get the maven tool.
   // ** NOTE: This 'M3' maven tool must be configured
   // **       in the global configuration.           
   def mvnHome = tool 'M3'

   // Mark the code build 'stage'....
   stage 'Build'
   
   // Run the maven build   
   // org.jenkinsci.plugins.scriptsecurity.sandbox.RejectedAccessException: Scripts not permitted to use method groovy.lang.GString plus java.lang.String
   //def buildCommand = "${mvnHome}" + '/bin/mvn -V  -P' + "${profile}" + ' clean package'
   
   //withEnv(['M2_HOME=/Users/jgaspard/Documents/DevTools/apache-maven-3.3.9']) {
        sh "${mvnHome}/bin/mvn -V clean"
   //}
   
   sh "git merge release-0.57.0"
   sh "git push --set-upstream origin develop"
   sh "git checkout -b master origin/master"
   sh "git merge release-0.57.0"
   sh "git push"
   
   sh "${mvnHome}/bin/mvn -V -Dusername=jgaspard-fa -Dpassword=git87=m -DnoReleaseMerge -DkeepBranch clean jgitflow:release-finish"
   
   sh "git checkout master"
   sh "git merge release-0.57.0"
   sh "git push"
   sh "git push origin --delete release-0.57.0"
   
   
   //if (profileTest)
   //     step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
}
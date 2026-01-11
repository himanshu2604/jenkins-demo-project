pipeline {
       agent any
       
       triggers {
           pollSCM('H/2 * * * *')  // Check Git every 2 minutes
       }
       
       stages {
           stage('Detect Branch') {
               steps {
                   script {
                       echo '========================================='
                       echo '🔍 Checking which branch was pushed...'
                       echo "Current branch: ${env.BRANCH_NAME}"
                       echo '========================================='
                   }
               }
           }
           
           stage('Trigger Test on Develop Push') {
               when {
                   branch 'develop'
               }
               steps {
                   script {
                       echo '========================================='
                       echo '✅ Push detected on develop branch!'
                       echo '🚀 Triggering test-job...'
                       echo '========================================='
                       
                       // Trigger test-job and wait for result
                       def testResult = build job: 'test-job', 
                                       parameters: [], 
                                       wait: true,
                                       propagate: false
                       
                       echo '========================================='
                       echo "📊 Test Job Result: ${testResult.result}"
                       echo '========================================='
                       
                       // Check if test job was successful
                       if (testResult.result == 'SUCCESS') {
                           echo '========================================='
                           echo '✅ Test job completed successfully!'
                           echo '🚀 Triggering production job...'
                           echo '========================================='
                           
                           // Trigger prod-job
                           def prodResult = build job: 'prod-job', 
                                           parameters: [], 
                                           wait: true
                           
                           echo '========================================='
                           echo "📊 Production Job Result: ${prodResult.result}"
                           echo '✅ Production deployment completed!'
                           echo '========================================='
                       } else {
                           echo '========================================='
                           echo '❌ Test job failed!'
                           echo '⚠️  Production deployment ABORTED!'
                           echo '========================================='
                           error 'Test job failed. Stopping pipeline.'
                       }
                   }
               }
           }
       }
       
       post {
           success {
               echo '========================================='
               echo '🎉 PIPELINE COMPLETED SUCCESSFULLY!'
               echo '========================================='
               echo '✓ Develop branch detected'
               echo '✓ Test job executed successfully'
               echo '✓ Production job executed successfully'
               echo '✓ Files copied to both environments'
               echo '========================================='
           }
           
           failure {
               echo '========================================='
               echo '❌ PIPELINE FAILED!'
               echo '========================================='
               echo 'Check console output for details'
               echo 'Possible issues:'
               echo '  - Test job failed'
               echo '  - Job names incorrect'
               echo '  - Git connectivity issues'
               echo '========================================='
           }
           
           always {
               echo '🏁 Pipeline execution finished'
               echo "Build Number: ${env.BUILD_NUMBER}"
               echo "Branch: ${env.BRANCH_NAME}"
           }
       }
   }

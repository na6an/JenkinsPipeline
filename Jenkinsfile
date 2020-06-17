pipeline {
     agent any
     stages {
         stage('Build') {
             steps {
                 sh 'echo "Hello World"'
                 sh '''
                     echo "Multiline shell steps works too"
                     ls -lah
                 '''
             }
         }
         stage('Lint HTML') {
              steps {
                  sh 'tidy -q -e *.html'
              }
         }

         stage('Dev Test') {
            when {
                branch 'dev' 
                }
              steps {
			sh 'echo %PATH%'
			sh 'chmod +x dev_script.sh'
            		sh './dev_script.sh'
                  withAWS(region:'us-east-2') {

            		s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, path:'P1_Deploy_Static_Website/', includePathPattern:'**/*', bucket:'nathan-udacity-pipeline')
            			}
              }
         }

        stage('Prod, upload given') {
            when {
                branch 'prod'
            }                
              steps {
                  withAWS(region:'us-east-2') {
                  sh 'echo "Uploading original html file to S3"'
                  s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'nathan-udacity-pipeline')
                  }
              }
         }
     }
}

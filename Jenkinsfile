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

         stage('Upload given, original html file to AWS') {

            when {
                branch 'dev' 

              steps {
                  withAWS(region:'us-east-2') {
		  git clone https://github.com/na6an/CDevOps.git
		  sh 'echo "Uploading P1 to S3"'
		  s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, path:'P1_Deploy_Static_Website/', includePathPattern:'**/*', file:'index.html', bucket:'nathan-udacity-pipeline')
			}
		}
              }
         }

        stage('Test - upload modified test html file to AWS') {
            when {
                branch 'prod' 
              steps {
                  withAWS(region:'us-east-2') {
                  sh 'echo "Uploading to S3"'
                  s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'nathan-udacity-pipeline')
                  }
              }
            }
         }

     }
}

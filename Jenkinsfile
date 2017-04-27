pipeline {
   agent none
      stages {
        stage ('Parallel Tests') { 
          steps {
            parallel(
              "Build Node-01" : {
              node('node-01'){
               sh '''#!/bin/bash
                    env
                    pwd
                  '''
              }
            }
        )
      }
    }
  }
}



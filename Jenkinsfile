pipeline {
   agent none
      stages {
        stage ('Parallel Tests') { 
          steps {
            parallel(
              "Build Node-01" : {
              node('node-01'){
               sh '''#!/bin/bash
                     cd /home/lisk/jenkins/workspace/
                     git clone https://github.com/LiskHQ/lisk.git
                     cd lisk
                     git checkout $BRANCH_NAME
                     pkill -f app.js || true
                     dropdb lisk_test
                     createdb lisk_test
                     psql -d lisk_test -c "alter user "$USER" with password 'password';"
                     cp ~/lisk-node-Linux-x86_64.tar.gz .
                     tar -zxvf lisk-node-Linux-x86_64.tar.gz
                     npm install
                     git submodule init
                     git submodule update
                     cd public
                     npm install
                     bower install
                     grunt release
                     cd ..
                     cd test/lisk-js/; npm install; cd ../..
                     cp test/config.json test/genesisBlock.json .
                     BUILD_ID=dontKillMe ~/start_lisk.sh
                  '''
              }
            }
        )
      }
    }
  }
}



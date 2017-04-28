pipeline {
   agent { label 'master' }
      stages {
           stage ('Node Provisioning') { 
                steps {
                  parallel(
                    "Build Node-01" : {
                      node('node-01'){
                       sh '''#!/bin/bash
                             cd /home/lisk/jenkins/workspace/
                             // Cleanup steps in case the job failed previously
                             pkill -f app.js || true
                             rm -rf lisk
                             git clone https://github.com/LiskHQ/lisk.git
                             cd lisk
                             git checkout $BRANCH_NAME
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
                    },
                    "Build Node-02" : {
                      node('node-02'){
                       sh '''#!/bin/bash
                             cd /home/lisk/jenkins/workspace/
                             // Cleanup steps in case the job failed previously
                             pkill -f app.js || true
                             rm -rf lisk
                             git clone https://github.com/LiskHQ/lisk.git
                             cd lisk
                             git checkout $BRANCH_NAME
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
                    },
                    "Build Node-03" : {
                      node('node-03'){
                       sh '''#!/bin/bash
                             cd /home/lisk/jenkins/workspace/
                             // Cleanup steps in case the job failed previously
                             pkill -f app.js || true
                             rm -rf lisk
                             git clone https://github.com/LiskHQ/lisk.git
                             cd lisk
                             git checkout $BRANCH_NAME
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
           stage ('Parallel Tests') { 
               steps {
                 parallel(
                   "ESLint" : {
                     node('node-01'){
                      sh '''
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run eslint
                      '''
                     }
                   },
                   "Functional Accounts" : {
                     node('node-01'){
                      sh '''
                      export TEST=test/api/accounts.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Blocks" : {
                     node('node-01'){
                      sh '''
                      export TEST=test/api/blocks.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                    '''
                     }
                   },
                   "Functional Delegates" : {
                     node('node-01'){
                      sh '''
                      export TEST=test/api/delegates.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Dapps" : {
                     node('node-01'){
                      sh '''
                      export TEST=test/api/dapps.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Loader" : {
                     node('node-01'){
                      sh '''
                      export TEST=test/api/loader.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Multisignatures" : {
                     node('node-01'){
                      sh '''
                      export TEST=test/api/multisignatures.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Signatures" : {
                     node('node-01'){
                      sh '''
                      export TEST=test/api/signatures.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Transactions" : {
                     node('node-01'){
                      sh '''
                      export TEST=test/api/transactions.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Peer" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Dapp" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.dapp.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Blocks" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.blocks.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Signatures" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.signatures.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Transactions Collision" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.transactions.collision.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Transactions Delegates" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.transactions.delegates.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Transactions Main" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.transactions.main.js  TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Transaction Signatures" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.transactions.signatures.js  TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Peers" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peers.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Votes" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.transactions.votes.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Unit - Helpers" : {
                     node('node-03'){
                      sh '''
                      export TEST=test/unit/helpers TEST_TYPE='UNIT'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Unit - Modules" : {
                     node('node-03'){
                      sh '''
                      export TEST=test/unit/modules TEST_TYPE='UNIT'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Unit - SQL" : {
                     node('node-03'){
                      sh '''
                      export TEST=test/unit/sql TEST_TYPE='UNIT'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Unit - Logic" : {
                     node('node-03'){
                      sh '''
                      export TEST=test/unit/logic TEST_TYPE='UNIT'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Stress - Transactions" : {
                     node('node-03'){
                      sh '''
                      export TEST=test/api/peer.transactions.stress.js TEST_TYPE='FUNC'
                      cd /home/lisk/jenkins/workspace/lisk
                      npm run jenkins
                      '''
                     }
                  }
               )
            }
         }
         stage ('Generate Coverage') {     
           steps {
             parallel(
               "Coverage Node-01" : {
                 node('node-01'){
                   sh '''#!/bin/bash
                     export HOST=127.0.0.1:4000
                     cd /home/lisk/jenkins/workspace/lisk
                   	npm run fetchCoverage
                      '''
                     }
                   },
                 "Coverage Node-02" : {
                   node('node-02'){
                     sh '''#!/bin/bash
                     export HOST=127.0.0.1:4000
                     cd /home/lisk/jenkins/workspace/lisk
                     npm run fetchCoverage
                     '''
                     }
                 },
                 "Coverage Node-03" : {
                   node('node-03'){
                     sh '''#!/bin/bash
                     export HOST=127.0.0.1:4000
                     cd /home/lisk/jenkins/workspace/lisk
                     npm run fetchCoverage
                     '''
                 }
               }
            )
          }
        }
        stage ('Gather Coverage') {
            steps {
               node('master'){
                  sh '''rm -rf coverage || true
                     mkdir coverage
                     cd coverage
                     // Fetching unit test coverage
                     scp lisk@node-01:/home/lisk/jenkins/workspace/lisk/test/.coverage-unit/lcov.info ./.coverage-unit/lcov.info
                     
                     // Fetching Functional results
                     scp lisk@node-01:/home/lisk/jenkins/workspace/lisk/test/.coverage-func.zip coverage-func-node-01.zip
                     scp lisk@node-02:/home/lisk/jenkins/workspace/lisk/test/.coverage-func.zip coverage-func-node-02.zip
                     scp lisk@node-03:/home/lisk/jenkins/workspace/lisk/test/.coverage-func.zip coverage-func-node-03.zip
                     '''
               }
            }
        }
        stage ('Combine Coverage') {
            steps {
               node('master'){
                  sh '''cd /var/lib/jenkins/coverage-scripts
                     bash after-success.sh
                     rm -rf /var/lib/jenkins/coverage/*
                     '''
               }
            }
        }        
        stage ('Node Cleanup') {     
           steps {
             parallel(
               "Cleanup Node-01" : {
                 node('node-01'){
                   sh '''
                     pkill -f app.js -9 
                      '''
                     }
                   },
                 "Cleanup Node-02" : {
                   node('node-02'){
                     sh '''
                     pkill -f app.js -9 
                     '''
                     }
                 },
                 "Cleanup Node-03" : {
                   node('node-03'){
                     sh '''
                     pkill -f app.js -9 
                     '''
                 }
               }
            )
          }
        }
    }
}


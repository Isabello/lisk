pipeline {
   agent none
      stages {
        stage ('Node Provisioning') { 
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
              },
              "Build Node-02" : {
                node('node-02'){
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
              },
              "Build Node-03" : {
                node('node-03'){
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
      stage ('Node Cleanup') {     
        steps {
              parallel(
                "Cleanup Node-01" : {
                  node('node-01'){
                   sh '''#!/bin/bash
                   pkill -f app.js -9 
                   rm -rf /home/lisk/jenkins/workspace/lisk
                   '''
                  }
                },
                "Cleanup Node-02" : {
                  node('node-02'){
                   sh '''#!/bin/bash
                   pkill -f app.js -9 
                   rm -rf /home/lisk/jenkins/workspace/lisk
                  '''
                  }
                },
                "Cleanup Node-03" : {
                  node('node-03'){
                   sh '''#!/bin/bash
                   pkill -f app.js -9 
                   rm -rf /home/lisk/jenkins/workspace/lisk
                  '''
                  }
               }
            )
        }
      }        
    }
  }
}



node('node-01'){
  lock(resource: "node-01", inversePrecedence: true) {
    stage ('Prepare Workspace') {
      sh '''#!/bin/bash
      pkill -f app.js || true
      '''
      deleteDir()
      checkout scm
    }

    stage ('Build Dependencies') {
      try {
        sh '''#!/bin/bash

        # Install Deps
        npm install

        # Install Nodejs
        tar -zxf ~/lisk-node-Linux-x86_64.tar.gz
        '''
      } catch (err) {
        currentBuild.result = 'FAILURE'
        error('Stopping build, installation failed')
      }
    }

    stage ('Build submodules') {
      try {
        sh '''#!/bin/bash
        git submodule init
        git submodule update
        cd public/
        npm install
        bower install
        grunt release
        '''
      } catch (err) {
        currentBuild.result = 'FAILURE'
        error('Stopping build, webpack failed')
      }
    }

    stage ('Setup test environemnt') {
      try {
        sh '''#!/bin/bash
        cd test/lisk-js/; npm install; cd ../..
        cp test/config.json test/genesisBlock.json .
        export NODE_ENV=test
        BUILD_ID=dontKillMe ~/start_lisk.sh
        '''
      } catch (err) {
        currentBuild.result = 'FAILURE'
        error('Stopping build, webpack failed')
      }
    }

    stage ('Parallel Tests') {
      parallel(
        "ESLint" : {
          node('node-01'){
           sh '''

           npm run eslint
           '''
          }
        },
        "Functional Accounts" : {
          node('node-01'){
           sh '''
           export TEST=test/api/accounts.js TEST_TYPE='FUNC'

           npm run jenkins
           '''
          }
        },
        "Functional Blocks" : {
          node('node-01'){
           sh '''
           export TEST=test/api/blocks.js TEST_TYPE='FUNC'

           npm run jenkins
         '''
          }
        },
        "Functional Delegates" : {
          node('node-01'){
           sh '''
           export TEST=test/api/delegates.js TEST_TYPE='FUNC'

           npm run jenkins
           '''
          }
        },
        "Functional Dapps" : {
          node('node-01'){
           sh '''
           export TEST=test/api/dapps.js TEST_TYPE='FUNC'

           npm run jenkins
           '''
          }
        },
        "Functional Loader" : {
          node('node-01'){
           sh '''
           export TEST=test/api/loader.js TEST_TYPE='FUNC'

           npm run jenkins
           '''
          }
        },
        "Functional Multisignatures" : {
          node('node-01'){
           sh '''
           export TEST=test/api/multisignatures.js TEST_TYPE='FUNC'

           npm run jenkins
           '''
          }
        },
        "Functional Signatures" : {
          node('node-01'){
           sh '''
           export TEST=test/api/signatures.js TEST_TYPE='FUNC'

           npm run jenkins
           '''
          }
        },
        "Functional Transactions" : {
          node('node-01'){
           sh '''
           export TEST=test/api/transactions.js TEST_TYPE='FUNC'

           npm run jenkins
           '''
        }
      })
    }

    stage ('Gather Coverage') {
      sh '''#!/bin/bash
      export HOST=127.0.0.1:4000

      npm run fetchCoverage

      # Submit coverage reports to Master
      scp test/.coverage-unit/lcov.info master-01:/var/lib/jenkins/coverage/.coverage-unit/lcov.info
      scp test/.coverage-func.zip master-01:/var/lib/jenkins/coverage/coverage-func-node-01.zip
      '''
    }

    stage ('Node-01 Cleanup') {
      sh '''
      pkill -f app.js -9
      '''
      }

    stage ('Set milestone') {
      milestone 1
      currentBuild.result = 'SUCCESS'
    }
  }
}

/*

           s,
                   "Functional Peer - Peer" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.js TEST_TYPE='FUNC'

                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Dapp" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.dapp.js TEST_TYPE='FUNC'

                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Blocks" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.blocks.js TEST_TYPE='FUNC'

                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Signatures" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.signatures.js TEST_TYPE='FUNC'

                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Transactions Collision" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.transactions.collision.js TEST_TYPE='FUNC'

                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Transactions Delegates" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.transactions.delegates.js TEST_TYPE='FUNC'

                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Transactions Main" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.transactions.main.js  TEST_TYPE='FUNC'

                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Transaction Signatures" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.transactions.signatures.js  TEST_TYPE='FUNC'

                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Peers" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peers.js TEST_TYPE='FUNC'

                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Peer - Votes" : {
                     node('node-02'){
                      sh '''
                      export TEST=test/api/peer.transactions.votes.js TEST_TYPE='FUNC'

                      npm run jenkins
                      '''
                     }
                   },
                   "Unit - Helpers" : {
                     node('node-03'){
                      sh '''
                      export TEST=test/unit/helpers TEST_TYPE='UNIT'

                      npm run jenkins
                      '''
                     }
                   },
                   "Unit - Modules" : {
                     node('node-03'){
                      sh '''
                      export TEST=test/unit/modules TEST_TYPE='UNIT'

                      npm run jenkins
                      '''
                     }
                   },
                   "Unit - SQL" : {
                     node('node-03'){
                      sh '''
                      export TEST=test/unit/sql TEST_TYPE='UNIT'

                      npm run jenkins
                      '''
                     }
                   },
                   "Unit - Logic" : {
                     node('node-03'){
                      sh '''
                      export TEST=test/unit/logic TEST_TYPE='UNIT'

                      npm run jenkins
                      '''
                     }
                   },
                   "Functional Stress - Transactions" : {
                     node('node-03'){
                      sh '''
                      export TEST=test/api/peer.transactions.stress.js TEST_TYPE='FUNC'

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

                   	npm run fetchCoverage
                      '''
                     }
                   },
                 "Coverage Node-02" : {
                   node('node-02'){
                     sh '''#!/bin/bash
                     export HOST=127.0.0.1:4000

                     npm run fetchCoverage
                     '''
                     }
                 },
                 "Coverage Node-03" : {
                   node('node-03'){
                     sh '''#!/bin/bash
                     export HOST=127.0.0.1:4000

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
                  sh '''rm -rf /var/lib/jenkins/coverage || true
                     mkdir -p /var/lib/jenkins/coverage/.coverage-unit
                     cd /var/lib/jenkins/coverage
                     scp lisk@node-01:/home/lisk/jenkins/workspace/lisk/test/.coverage-unit/lcov.info /var/lib/jenkins/coverage/.coverage-unit/lcov.info

                     scp lisk@node-01:/home/lisk/jenkins/workspace/lisk/test/.coverage-func.zip /var/lib/jenkins/coverage/coverage-func-node-01.zip
                     scp lisk@node-02:/home/lisk/jenkins/workspace/lisk/test/.coverage-func.zip /var/lib/jenkins/coverage/coverage-func-node-02.zip
                     scp lisk@node-03:/home/lisk/jenkins/workspace/lisk/test/.coverage-func.zip /var/lib/jenkins/coverage/coverage-func-node-03.zip
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



*/

pipeline {
    agent { label "builder" }
    options { disableConcurrentBuilds() }
    stages {
        stage("Checkout") {
          steps {
            cleanWs()
            checkout([$class: 'GitSCM', branches: [[name: '$BRANCH_NAME']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'github', url: 'git@github.com:amilabs/incubator-echarts.git']]])
          }
        }
        stage("Publish") {
          when {
              expression {
                  return sh(script: 'git tag --points-at | grep "^v" || true', returnStdout: true).trim().length() > 0
              }
          }
          steps {
                script{
                    withCredentials([string(credentialsId: 'xereverex-github-access-token', variable: 'NPM_TOKEN')]) {
                        sh 'npm i'
                        sh 'rm -rf .npmrc'
                        sh 'cp .npmrc.publish .npmrc'
                        sh "npm publish"
                    }
                }
          }
        }
    }
}

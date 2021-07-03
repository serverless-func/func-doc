pipeline {
  agent any
  environment {
    ACCOUNT_ID = '1607652824904310'
    REGION    = 'cn-hangzhou'
  }
  stages {
    stage("检出代码") {
      steps {
        checkout([
          $class: 'GitSCM',
          branches: [[name: env.GIT_BUILD_REF]],
          userRemoteConfigs: [[
            url: env.GIT_REPO_URL,
            credentialsId: env.CREDENTIALS_ID
        ]]])
      }
    }
    stage('写入版本号') {
      steps {
        sh '''
            sed -i "s/VERSION/${GIT_TAG}/g" slate/source/index.html.md
        '''
      }
    }

    stage('构建Slate') {
      steps {
        script {
            withCredentials([usernamePassword(credentialsId: 'd2d0211a-5f3f-46f6-92cd-d2ec9692bc27', usernameVariable: 'REGISTRY_USER', passwordVariable: 'REGISTRY_PASS')]) {
              sh "echo ${REGISTRY_PASS} | docker login -u ${REGISTRY_USER} --password-stdin dongfg-docker.pkg.coding.net"
            }
            imageFullName = "dongfg-docker.pkg.coding.net/serverless-aliyun/fun-doc/slate-builder:1.0.1"
            imageNotExists = sh(script: "DOCKER_CLI_EXPERIMENTAL=enabled docker manifest inspect $imageFullName > /dev/null", returnStatus: true)
            if (imageNotExists) {
                docker.build("$imageFullName", "./slate")
                sh "docker push $imageFullName"
            } else {
                docker.image(imageFullName)
            }

            sh "docker run --rm -v ${pwd()}/slate/source:/srv/slate/source -v ${pwd()}/static:/srv/slate/build $imageFullName"
        }
      }
    }

    stage('构建Bootstrap') {
      steps {
        sh '''
            mv static .deploy/
            go build -o .deploy/bootstrap .deploy/main.go
        '''
      }
    }
    stage('下载证书') {
      steps {
        script {
            sh '''
              curl -fL "https://dongfg-generic.pkg.coding.net/serverless-aliyun/secrets/ssl.fun.key?version=latest" -o ./.deploy/ssl.private.pem
              curl -fL "https://dongfg-generic.pkg.coding.net/serverless-aliyun/secrets/ssl.fun.cer?version=latest" -o ./.deploy/ssl.cert.pem
            '''
        }
      }
    }
    stage('部署') {
      when {
        buildingTag()
      }
      steps {
        script {
          try {
            withCredentials([usernamePassword(credentialsId: '6384c2ec-5a4d-4b6c-ad57-992dc5a88842', usernameVariable: 'ACCESS_KEY_ID', passwordVariable: 'ACCESS_KEY_SECRET')]) {
              sh "curl -o fun-linux.zip http://funcruft-release.oss-accelerate.aliyuncs.com/fun/fun-v3.6.21-linux.zip"
              sh "unzip fun-linux.zip"
              sh "mv fun-v3.6.21-linux /usr/local/bin/fun"
              sh "cd .deploy/ && fun deploy -y"
            }
          } catch(err) {
            echo err.getMessage()
          }
        }
      }
    }
  }
}
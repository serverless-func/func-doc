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
            imageFullName = "dongfg-docker.pkg.coding.net/serverless-aliyun/fun-doc/slate-builder:1.0.0"
            imageNotExists = sh(script: "DOCKER_CLI_EXPERIMENTAL=enabled docker manifest inspect $imageFullName > /dev/null", returnStatus: true)
            if (imageNotExists) {
                docker.build("$imageFullName", "./slate")
                sh "docker push $imageFullName"
            } else {
                docker.image(imageFullName)
            }

            sh "docker run --rm -v ${pwd()}/static:/srv/slate/build $imageFullName"
        }
      }
    }



    stage('构建Bootstrap') {
      steps {
        sh '''
            go build -o bootstrap
            mkdir artifact
            mv static artifact/
            mv bootstrap artifact/
        '''
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
              sh "npm install @alicloud/fun -g"
              sh "fun deploy -y"
            }
          } catch(err) {
            echo err.getMessage()
          }
        }
      }
    }
  }
}
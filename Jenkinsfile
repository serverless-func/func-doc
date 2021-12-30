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
            imageFullName = "dongfg-docker.pkg.coding.net/serverless-aliyun/fun-doc/slate-builder:1.1.1"
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

    stage('部署') {
      when {
        buildingTag()
      }
      steps {
        script {
          withCredentials([usernamePassword(credentialsId: 'e8d95aac-7e13-432e-83ff-0f157cc1afc8', usernameVariable: 'USER', passwordVariable: 'ACCESS_TOKEN')]) {
            sh """
              cd static
              git init
              git config --local user.name dongfg
              git config --local user.email mail@dongfg.com
              git remote add origin https://${ACCESS_TOKEN}@github.com/serverless-aliyun/fun-doc.git
              git checkout -b gh-pages
              git add --all
              git commit -m "deploy gh-pages"
              git push origin gh-pages -f
            """
          }
        }
      }
    }
  }
}
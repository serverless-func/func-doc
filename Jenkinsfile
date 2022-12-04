pipeline {
  agent any
  environment {
    ACCOUNT_ID = '1607652824904310'
    REGION    = 'cn-hangzhou'
    CURRENT_TIME = sh(returnStdout: true, script: 'date +"%y-%m-%d %T"').trim()
    DOCKER_CACHE_EXISTS = fileExists '/root/.cache/docker/slatedocs-slate-latest.tar'
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
            sed -i "s/VERSION/${GIT_TAG}/g" source/index.html.md
            sed -i "s/BUILD_TIME/${CURRENT_TIME}/g" source/index.html.md
        '''
      }
    }

    stage('加载缓存') {
      when { expression { DOCKER_CACHE_EXISTS == 'true' } }
      steps {
        sh 'docker load -i /root/.cache/docker/slatedocs-slate-latest.tar'
      }
    }

    stage('构建Slate') {
      agent {
        docker {
          image 'slatedocs/slate'
          args '-v /root/workspace/source:/srv/slate/source -v /root/workspace/static:/srv/slate/build'
          reuseNode true
        }
      }
      steps {
        sh 'cd /srv/slate && /srv/slate/slate.sh build -v'
      }
    }

    stage('生成缓存') {
      when { expression { DOCKER_CACHE_EXISTS == 'false' } }
      steps {
        sh 'mkdir -p /root/.cache/docker/'
        sh 'docker save -o /root/.cache/docker/slatedocs-slate-latest.tar slatedocs/slate'
      }
    }

    stage('部署') {
      when {
        buildingTag()
      }
      steps {
        script {
          withCredentials([usernamePassword(credentialsId: 'fa9ddf3e-8048-498b-9e36-9303fe649dba', usernameVariable: 'USER', passwordVariable: 'ACCESS_TOKEN')]) {
            sh """
              cd static
              echo "func.dongfg.com" > CNAME
              git init
              git config --local user.name dongfg
              git config --local user.email mail@dongfg.com
              git remote add origin https://${ACCESS_TOKEN}@github.com/serverless-func/func-doc.git
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
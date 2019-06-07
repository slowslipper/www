def basePath = "/"

node {
  stage ('project name') {
    println "PROJECT_NAME: $PROJECT_NAME"
  }
  stage ('deploy path') {
    try {
      basePath = "/$DEPLOY_PATH/"
      println basePath
    }
    catch (MissingPropertyException e) {
      println 'none'
    }
  }
  stage ('clone') {
    git 'https://github.com/slowslipper/www.git'
  }
  dir (PROJECT_NAME) {
    stage ('install') {
      sh 'yarn install'
    }
    stage ('build') {
      sh 'yarn build'
    }
    stage ('upload') {
      withAWS(region: 'ap-northeast-2', credentials: '726a3bed-0458-4929-891c-2d7e3425add5') {
        def identity=awsIdentity();
        s3Upload(bucket: 'slowslipper.com', workingDir:'dist', path: DEPLOY_PATH, includePathPattern:'**/*');
      }
    }
    stage ('invalidate') {
      withAWS(region: 'ap-northeast-2', credentials: '726a3bed-0458-4929-891c-2d7e3425add5') {
        def identity=awsIdentity();
        cfInvalidate(distribution: 'E1C8SX5W4T34XE', paths: ["${basePath}*"]);
      }
    }
  }
}
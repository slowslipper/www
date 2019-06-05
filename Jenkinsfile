node {
  stage ('clone') {
    git 'https://github.com/slowslipper/www.git'
  }
  stage ('install') {
    sh 'yarn install'
  }
  stage ('build') {
    sh 'yarn build'
  }
  stage ('upload') {
    withAWS(region: 'ap-northeast-2', credentials: '726a3bed-0458-4929-891c-2d7e3425add5') {
      def identity=awsIdentity();
      s3Upload(bucket: 'slowslipper.com', workingDir:'dist', includePathPattern:'**/*');
    }
  }
  stage ('invalidate') {
    withAWS(region: 'ap-northeast-2', credentials: '726a3bed-0458-4929-891c-2d7e3425add5') {
      def identity=awsIdentity();
      cfInvalidate(distribution: 'E1C8SX5W4T34XE', paths: ['/index.html']);
    }
  }
}
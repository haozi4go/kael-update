pipeline {
  agent any
  environment { 
    update_project_type = 'jar' //war zip jar
  }
  //options {
    //disableConcurrentBuilds() //不允许并行执行Pipeline
    //timestamps()
  //}
  parameters{
    choice(name: 'update_project', choices: 'pbase\nbill\ndemo\n', description: '请选择升级的应用')
    string(name: 'update_version', defaultValue: '', description: '升级指定版本,默认为最新版本')
    choice(name: 'update_host', choices: '140.143.246.85\n140.143.246.85\n', description: '请选择升级的目标主机')
  }
  stages {
    stage('Pull') {
      steps {
        //target_host = sh(returnStatus: true, script: "cut -d ':' -f1 ${update_host}")
        sh "ssh root@${update_host} 'su - ${update_project} ; sh ~/kael/update/pull.sh' "
        echo 'Pull success.'
      }
    }

    stage('Depoly') {
      steps {
        //target_host = sh(returnStatus: true, script: "cut -d ':' -f1 ${update_host}")
        sh "ssh root@${update_host} 'su - ${update_project} ; sh ~/kael/update/deploy.sh' "
        echo "Depoly success."
      }
    }
    
    stage('Restart') {
      steps {
        //target_host = sh(returnStatus: true, script: "cut -d ':' -f1 ${update_host}")
        sh "ssh root@${update_host} 'su - ${update_project} ; sh ~/kael/update/restart.sh' "
        echo "Restart success."
      }
    }
    
    stage('Logging') {
      steps {
        //target_host = sh(returnStatus: true, script: "cut -d ':' -f1 ${update_host}")
        sh "ssh root@${update_host} 'su - ${update_project} ; tail -100f ~/apps/logs/${update_project}.log' "
        echo "Logging success."
      }
    }

  }
}

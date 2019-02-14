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
    choice(name: 'update_host', choices: '140.143.246.85:test\n140.143.246.85:prod\n', description: '请选择升级的目标主机')
  }
  stages {
    stage('Pull') {
      steps {
        sh "ssh root@\$(echo ${update_host} | cut -d \":\" -f1) 'sh ~/kael/update/pull.sh ${update_project} ${update_version}' "
        echo 'Pull success.'
      }
    }

    stage('Depoly') {
      steps {
        sh "ssh root@\$(echo ${update_host} | cut -d \":\" -f1) 'sh ~/kael/update/deploy.sh ${update_project} ${update_version}' "
        echo "Depoly success."
      }
    }
    
    stage('Restart') {
      options {
        timeout(time: 2, unit: 'MINUTES') 
      }
      steps {
        sh "ssh root@\$(echo ${update_host} | cut -d \":\" -f1) 'sh ~/kael/update/restart.sh ${update_project} | sed \"/started/Q\" ' "  // 检测到指定内容started则退出
        echo "Restart success."
      }
    }
    
    stage('Logging') {
      options {
        timeout(time: 2, unit: 'MINUTES') 
      }
      steps {
        sh "ssh root@\$(echo ${update_host} | cut -d \":\" -f1) 'tail -100f /home/${update_project}/apps/logs/${update_project}.log | sed \"/Updating port to/Q\" ' " // 检测到指定内容Updating port to则退出
        echo "Logging success."
      }
    }
    stage('Check running') {
      steps {
        sh "ssh root@\$(echo ${update_host} | cut -d \":\" -f1) 'ps -ef|grep ${update_project} | grep -v grep' "
        echo "Check running success."
      }
    }

  }
}

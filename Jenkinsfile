pipeline {
  agent any
  environment { 
    update_project_type = 'jar' //war zip jar
    update_resart_time = '5' //
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
        //sh "ssh root@${update_host} 'sh ~/kael/update/pull.sh ${update_project} ${update_version}' "
        echo 'Pull success.'
      }
    }

    stage('Depoly') {
      steps {
        //target_host = sh(returnStatus: true, script: "cut -d ':' -f1 ${update_host}")
        //sh "ssh root@${update_host} 'sh ~/kael/update/deploy.sh ${update_project} ${update_version}' "
        echo "Depoly success."
      }
    }
    
    stage('Restart') {
      options {
        timeout(time: 2, unit: 'MINUTES') 
      }
      steps {
        //target_host = sh(returnStatus: true, script: "cut -d ':' -f1 ${update_host}")
        //script {
          //ssh root@${update_host} 'sh ~/kael/update/restart.sh ${update_project}'
          //ssh root@${update_host} 'exit 1'          
        //}
        sh "ssh root@${update_host} 'sh ~/kael/update/restart.sh ${update_project}; echo 'waitting...'; sleep ${update_resart_time}; exit 1 ' "
        echo "Restart success."
      }
    }
    
    stage('Logging') {
      options {
        timeout(time: 2, unit: 'MINUTES') 
      }
      steps {
        //target_host = sh(returnStatus: true, script: "cut -d ':' -f1 ${update_host}")
        sh "ssh root@${update_host} 'tail -100f /home/${update_project}/apps/logs/${update_project}.log' "
        echo "Logging success."
      }
    }

  }
}

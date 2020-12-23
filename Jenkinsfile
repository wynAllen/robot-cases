pipeline {
    agent {
        label 'jnlp-slave'
    }

	options {
        timeout(time: 20, unit: 'MINUTES')
		gitLabConnection('gitlab')
	}
    stages {
        stage('checkout') {
            steps {
                container('tools') {
                    checkout scm
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    container('tools'){
                        switch(env.comp){
                            case "myblog":
                                env.testDir = "myblog"
                                break
                            case "business1":
                                env.testDir = "business1"
                                break
                            default:
                                env.testDir = "all"
                                break
                        }
                        sh 'robot -d artifacts/ ${testDir}/*'
                        step([
                            $class : 'RobotPublisher',
                            outputPath: 'artifacts/',
                            outputFileName : "output.xml",
                            disableArchiveOutput : false,
                            passThreshold : 100,
                            unstableThreshold: 80.0,
                            onlyCritical : true,
                            otherFiles : "*.png"
                        ])
                        archiveArtifacts artifacts: 'artifacts/*', fingerprint: true
                    }
                }
            }
        }
    }
}
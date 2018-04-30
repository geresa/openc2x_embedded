def axisTargets = ["x86_64","x86_geode","ar71xx_generic"]
def checkout_tasks = [:]
def build_tasks = [:]
def archive_tasks = [:]

for(int i=0; i< axisTargets.size(); i++) {
    def target = axisTargets[i]
    def targets_split = target.tokenize( '_' )
    def maintarget = targets_split[0]
    def subtarget = targets_split[1]
    	checkout_tasks["${target}"] = {
		node {
			ws("${env.JOB_NAME}-${target}") {
				dir('firmware') {
					checkout scm
				}
			}
		}
	}
	build_tasks["${target}"] = {
		node {
			ws("${env.JOB_NAME}-${target}") {
				dir('firmware') {
					sh "./scripts/feeds update -a"
					sh "./scripts/feeds install -a"
					sh "./create_config.sh ${target}"
					sh "make defconfig"
					sh "make -j2 V=s"
				}
			}
		}
	}
	archive_tasks["${target}"] = {
		node {
			ws("${env.JOB_NAME}-${target}") {
				dir('firmware') {
					archiveArtifacts artifacts: 'bin/targets/*/*/*', excludes: ''
				}
			}
		}
	}
}

stage('Checkout') { 
	parallel checkout_tasks
}

stage ("Build") {
	parallel build_tasks
}

stage ("Archive") {
	parallel archive_tasks
}




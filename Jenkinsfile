pipeline {
	agent any	
    stages {
        stage('Build') {
			agent {
				docker {
					image 'maven:3-alpine'
					args '-v $HOME/.m2:/root/.m2'
				}
			}
            steps {
				sh 'mvn -B clean package'
				junit(allowEmptyResults: true, testResults: '**/target/surefire-reports/*.xml, **/target/failsafe-reports/*.xml')
                findbugs(pattern: '**/target/findbugs.xml,**/target/findbugsXml.xml', unstableTotalAll: '0', failedTotalAll: '0')
                pmd(pattern: '**/target/pmd.xml', failedTotalAll: '0', unstableTotalAll: '0')
                checkstyle(pattern: '**/target/checkstyle-result.xml', failedTotalAll: '0', unstableTotalAll: '0')
				archiveArtifacts(fingerprint: true, artifacts: '**/target/*.jar', excludes: '**/target/fb-contrib-*.jar, **/target/findsecbugs-plugin-*.jar')
            }
        }

        stage('Build Docker image of config-service') {
			steps {
				sh "mkdir -p config-service/target/docker/"
				sh "cp config-service/src/main/docker/Dockerfile config-service/target/docker/Dockerfile"
				sh "ls -lastr config-service/target"
				sh "cp config-service/target/config-service-0.0.1-SNAPSHOT.jar config-service/target/docker/config-service-0.0.1-SNAPSHOT.jar"
                sh "docker build -t config-service ./config-service/target/docker/ --build-arg SPRING_BOOT_FAT_JAR=config-service-0.0.1-SNAPSHOT.jar"
            }
        }
        
    }
}
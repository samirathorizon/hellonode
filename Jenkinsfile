pipeline {
    agent {
        kubernetes {
            yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          volumes:
            - name: build-cache
              persistentVolumeClaim: 
                claimName: build-cache
          serviceAccountName: jenkins-agents
          containers:
         - name: docker
            image: myreg/docker:1
            volumeMounts:
            - name: build-cache
              mountPath: /var/lib/docker
              subPath: docker
            command:
            - cat
            tty: true
            securityContext:
              privileged: true
       '''
        }
    }
    stages {

        // checkout and test

        stage('Build UI Docker Image') {
            steps {
                container('docker') {
                      sh 'dockerd & > /dev/null'
                      sleep(time: 10, unit: "SECONDS")
                      sh "docker build  -t myreg/myapp/ui:$BUILD_NUMBER ."
                      sh "docker push myreg/myapp/ui:$BUILD_NUMBER"
                }
            }
        }
    }
}

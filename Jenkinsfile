
node {
    docker.image('node:16-buster-slim').inside('-p 3000:3000') {

        withEnv(['CI=true']) {

            stage('Build') {
                sh 'npm install'
            }

            stage('Test') {
                sh './jenkins/scripts/test.sh'
            }

            stage('Manual Approval') {
                input message: 'Lanjutkan ke tahap Deploy?',
                      ok: 'Proceed'
            }

            stage('Deploy') {
                try {
                    echo 'Memulai deployment React App...'

                    sh './jenkins/scripts/deliver.sh'

                    echo 'Aplikasi dijalankan selama 60 detik...'

                    sh 'sleep 60'

                } finally {
                    echo 'Menghentikan React App...'

                    sh '''
                        if [ -f .pidfile ]; then
                            ./jenkins/scripts/kill.sh
                            rm -f .pidfile
                        else
                            echo "PID file tidak ditemukan."
                        fi
                    '''
                }
            }
        }
    }
}
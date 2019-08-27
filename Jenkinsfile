pipeline {
    agent { label 'master' }
    stages {
        stage('build') {
            steps {
                echo "Get kubectl"
                sh "curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.15.0/bin/linux/amd64/kubectl"
                sh "chmod +x ./kubectl"
                echo "Get Helm"
                sh "curl -LO https://get.helm.sh/helm-v2.14.3-linux-amd64.tar.gz"
                sh "tar xzf helm-v2.14.3-linux-amd64.tar.gz"
                sh "mv linux*/helm ."
                sh "chmod +x helm"
                sh "./helm init --client-only"
                echo "Init helm"
                sh "./helm fetch \
  --repo https://kubernetes-charts.storage.googleapis.com \
  --untar \
  --untardir ./charts \
  --version 5.5.3 \
    prometheus"
                // sh "mkdir values"
                // sh "mkdir manifests"
                echo "Generate manifests"
                sh "cp ./charts/prometheus/values.yaml ./values/prometheus.yaml"
                sh "./helm template \
  --values ./values/prometheus.yaml \
  --output-dir ./manifests \
    ./charts/prometheus"
            }
        }
    }
}

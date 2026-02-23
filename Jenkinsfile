pipeline {
    agent any

    parameters {
        choice(name: 'MODULE', choices: ['ems', 'cns', 'tms', 'wfh', 'amenitybooking'], description: 'Select the module')
        choice(name: 'BRANCH', choices: ['dev','feature', 'main'], description: 'Git branch name')
        choice(name: 'NAMESPACE', choices: ['spm-dev', 'spm-prod'], description: 'Namespace_name')
       //choice(name: 'JENKINS_PATH', choices: ['jumpreeqa','digitalstaging','integration','jumpreevapt','spacemanagement_gcp','ssdevcloud','staging','propertyguru'], description: 'Please select the appropriate namespace')
    }
    tools {
       jdk "JAVA_HOME"
       }
    environment {
        PATH="MAVEN_HOME:$PATH"
        CREDENTIALS_ID = 'abec083d-f2fd-4744-9b39-66f274d5275b'
        DOCKER_REGISTRY = 'jk2425'
        GCP_AUTH = credentials('gke-auth')
    }

    stages {
        stage('Set Repo') {
            steps {
                script {
                    def repoMap = [
                        ems: 'EMS',
                        cns: 'CommonNotificationService',
                        tms: 'TMS',
                        wfh: 'workfromhome',
                        amenitybooking: 'spacemanagement'
                    ]
                    env.REPO_NAME = repoMap[params.MODULE]
                    echo "📦 Selected repo: ${env.REPO_NAME} for branch: ${params.BRANCH}"
                }
            }
        }

        stage('Checkout Source') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: "${params.BRANCH}"]],
                    userRemoteConfigs: [[
                        url: "https://gitlab.smartenspaces.com/saas-smartenspaces/${env.REPO_NAME}.git",
                        credentialsId: env.CREDENTIALS_ID
                    ]]
                ])
            }
        }

        stage ('Maven Build') {
            steps {
               sh 'mvn clean install -DskipTests' 
               ///sh 'mvn project-info-reports:licenses'
               ///sh 'mvn project-info-reports:dependencies'
               sh "echo '${currentBuild.result}'"
            }
        }

        stage('Move JAR') {
            steps {
                script {
                    def jarSourcePath = "${env.WORKSPACE}/target/*.jar"
                    def jarTargetPath = "/var/lib/jenkins/mtdevold/${NAMESPACE}_gcp/${params.MODULE}"
                    
                    echo "🔍 Looking for JAR at: ${jarSourcePath}"

                    sh """
                if [ ! -f ${jarSourcePath} ]; then
                    echo '❌ JAR file not found: ${jarSourcePath}'
                    exit 1
                fi
                echo '✅ JAR file exists.'
                mkdir -p ${jarTargetPath}
                echo '♻️ Replacing old JAR (if any)...'
                mv -f ${jarSourcePath} ${jarTargetPath}/
                echo '✅ Moved new JAR to ${jarTargetPath}'
                ls -lh ${jarTargetPath}
                     """
                }
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    def contextPath = "/var/lib/jenkins/mtdevold/${NAMESPACE}_gcp/${params.MODULE}/"
                    def imageName = "${DOCKER_REGISTRY}/${params.MODULE}:gcpdev${NAMESPACE}${env.BUILD_NUMBER}"

                    echo "🐳 Docker build context: ${contextPath}"
                    echo "📤 Docker image name: ${imageName}"

                    sh """
                        cd ${contextPath}
                        docker build -t ${params.MODULE} -f Dockerfile .
                        docker tag $MODULE ${imageName}
                        docker push ${imageName}
                        gcloud auth activate-service-account --key-file=$GCP_AUTH
                        gcloud config set project ss-dev-infra-01
                        gcloud container clusters get-credentials ss-gcp-dev-singapore-cluster-01 --region asia-southeast1
                        kubectl set image deployment ${params.MODULE} ${params.MODULE}=jk2425/${params.MODULE}:gcpdev${NAMESPACE}${env.BUILD_NUMBER} -n $NAMESPACE --record=true
                    """
                }
            }
        }
    }

    post {
        failure {
            echo "❌ Build failed for module ${params.MODULE}. Check logs above for errors."
        }
        success {
            echo "✅ Build, JAR transfer, and Docker push succeeded for ${params.MODULE} 🎉"
        }
    }
}


---


pipeline {
    agent any

    parameters {
        choice(name: 'MODULE', choices: ['ems', 'cns', 'tms', 'wfh', 'amenitybooking'], description: 'Select the module')
        choice(name: 'BRANCH', choices: ['dev','feature', 'main'], description: 'Git branch name')
        choice(name: 'NAMESPACE', choices: ['spm-dev', 'spm-prod'], description: 'Target Namespace')
    }

    tools {
        jdk "JAVA_HOME"
        maven "MAVEN_HOME"
    }

    environment {
        CREDENTIALS_ID = 'abec083d-f2fd-4744-9b39-66f274d5275b'
        DOCKER_REGISTRY = 'jk2425'
        GCP_AUTH = credentials('gke-auth')
        SONAR_TOKEN = credentials('sonar-token') // Added for SAST
        IMAGE_TAG = "gcpdev-${params.NAMESPACE}-${env.BUILD_NUMBER}"
        FULL_IMAGE_NAME = "${DOCKER_REGISTRY}/${params.MODULE}:${IMAGE_TAG}"
    }

    stages {
        stage('Checkout & Setup') {
            steps {
                script {
                    def repoMap = [ems:'EMS', cns:'CommonNotificationService', tms:'TMS', wfh:'workfromhome', amenitybooking:'spacemanagement']
                    env.REPO_NAME = repoMap[params.MODULE]
                }
                checkout([$class: 'GitSCM', branches: [[name: "${params.BRANCH}"]], 
                    userRemoteConfigs: [[url: "https://gitlab.smartenspaces.com/saas-smartenspaces/${env.REPO_NAME}.git", credentialsId: env.CREDENTIALS_ID]]])
            }
        }

        stage('SAST - SonarQube Scan') {
            steps {
                // DevSecOps Standard: Static Analysis before building
                echo "🔍 Running Static Code Analysis..."
                sh "mvn sonar:sonar -Dsonar.projectKey=${params.MODULE} -Dsonar.login=${SONAR_TOKEN}"
            }
        }

        stage('Build & Unit Test') {
            steps {
                // DevSecOps Standard: Running tests instead of -DskipTests
                sh 'mvn clean install' 
            }
        }

        stage('Docker Build & Vulnerability Scan') {
            steps {
                script {
                    sh "docker build -t ${FULL_IMAGE_NAME} ."
                    
                    // DevSecOps Standard: Scan image before pushing to Registry
                    echo "🛡️ Scanning Image for Vulnerabilities with Trivy..."
                    sh "trivy image --severity HIGH,CRITICAL --exit-code 1 ${FULL_IMAGE_NAME}"
                }
            }
        }

        stage('Push to Registry') {
            steps {
                sh "docker push ${FULL_IMAGE_NAME}"
            }
        }

        stage('GKE Deployment') {
            steps {
                script {
                    sh """
                        gcloud auth activate-service-account --key-file=$GCP_AUTH
                        gcloud config set project ss-dev-infra-01
                        gcloud container clusters get-credentials ss-gcp-dev-singapore-cluster-01 --region asia-southeast1
                        kubectl set image deployment/${params.MODULE} ${params.MODULE}=${FULL_IMAGE_NAME} -n ${params.NAMESPACE} --record=true
                    """
                }
            }
        }

        stage('Health Check & Auto-Rollback') {
            steps {
                script {
                    echo "⏳ Waiting for pods to stabilize (5-minute window)..."
                    // Wait for deployment to finish or timeout
                    def status = sh(script: "kubectl rollout status deployment/${params.MODULE} -n ${params.NAMESPACE} --timeout=300s", returnStatus: true)
                    
                    if (status != 0) {
                        echo "⚠️ Deployment Failed/Timed out! Initiating Rollback..."
                        sh "kubectl rollout undo deployment/${params.MODULE} -n ${params.NAMESPACE}"
                        error("Deployment rolled back due to unhealthy pods.")
                    } else {
                        echo "✅ Deployment Successful: All pods are 1/1 Running."
                    }
                }
            }
        }
    }

    post {
        success {
            echo "🎉 Build and Deployment Successful for ${params.MODULE}"
            // Optional: slackSend (channel: '#deployments', message: "SUCCESS: ${params.MODULE} deployed to ${params.NAMESPACE}")
        }
        failure {
            echo "❌ Pipeline Failed. Checking for cleanup..."
            // Logic to clean up workspace or notify team
        }
    }
}

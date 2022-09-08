pipeline {
    agent any
    environment {
         // GIT URL
         GIT_URL="https://github.com/songweiaaa/sw.git"

         // Credential ID needs to be updated accordingly for different Jenkins env.
         GIT_CREDENTIALS = "ddd"

        SW_FOLDER = "${WORKSPACE}/BUILD_${BUILD_ID}/${params.ENBD_Switch}/"

        ENBD_Switch_Collection_folder = "${SW_FOLDER}" + "sw-info/"

    }
    stages {
         stage("SCM sync up") {
             steps {
                 dir("${WORKSPACE}") {
                     git branch: "main", credentialsId: "${GIT_CREDENTIALS}", url: "${GIT_URL}"
                 }
             }
         }
        stage("configure NTP") {
            steps {
			    echo 'configure NTP'
                sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/test-enbd-ntp.yaml"
                // sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/test-enbd-ntp.yaml -e hostlist=${params.ENBD_Switch}"

            }
        }
        stage("Collect cumulus files") {
            steps {
			    echo 'Collectting cumulus files'
                sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/test-enbd-collect-files.yaml -e hostlist=${params.ENBD_Switch} -e output_dir=${ENBD_Switch_Collection_folder}"
            }
        }

    }
}

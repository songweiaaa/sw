pipeline {
    agent any
     // parameters {
     //     choice(
     //         name: "ENBD_Switch",
     //         choices: ["vxlan-leaf01", "vxlan-leaf02", "vxlan-leaf03", "vxlan-leaf04"],
     //         description: "Select the (MLAG) primary switch in a ToR pair."
     //     )
     // }
    environment {
         // GIT URL
         GIT_URL="https://github.com/songweiaaa/sw.git"

         // Credential ID needs to be updated accordingly for different Jenkins env.
         GIT_CREDENTIALS = "ddd"

        SW_FOLDER = "${WORKSPACE}/BUILD_${BUILD_ID}/${params.ENBD_Switch}/"

        ENBD_Switch_Collection_folder = "${SW_FOLDER}" + "pre_upgrade_folder/"

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
            // input {
            //     message "Ready to continue?"
            //     ok "Yessss"
            // }
            steps {
			    echo 'configure NTP'
                sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/test-enbd-ntp.yaml -e hostlist=${params.ENBD_Switch}"
            }
        }
        stage("Collect cumulus files") {
            // input {
            //     message "Ready to continue?"
            //     ok "Yes"
            // }
            steps {
			    echo 'Collectting cumulus files'
                sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/test-enbd-collect-files.yaml -e hostlist=${params.ENBD_Switch} -e output_dir=${ENBD_Switch_Collection_folder}"
                // sh "/usr/bin/python3 ${WORKSPACE}/scripts/create_sw_snapshots.py -i ${SEC_SW_PRE_UPG_FOLDER} -o ${SEC_SW_PRE_UPG_SNAPSHOT}"
            }
        }

    }
}

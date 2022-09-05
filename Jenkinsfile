pipeline {
    agent any
     // parameters {
     //     choice(
     //         name: "Primary_Switch",
     //         choices: ["vxlan-leaf01", "vxlan-leaf02", "vxlan-leaf03", "vxlan-leaf04"],
     //         description: "Select the (MLAG) primary switch in a ToR pair."
     //     )
     //     choice(
     //         name: "Secondary_Switch",
     //         choices: ["vxlan-leaf01", "vxlan-leaf02", "vxlan-leaf03", "vxlan-leaf04"],
     //         description: "Select the (MLAG) secondary switch in a ToR pair."
     //     )
     //     choice(
     //         name: "Cumulus_OS",
     //         choices: [
     //             "cumulus-linux-3.7.16-vx-amd64.bin",
     //             "cumulus-linux-4.2.1-vx-amd64.bin",
     //             "cumulus-linux-4.3.0-vx-amd64.bin",
     //             "cumulus-linux-4.4.3-vx-amd64.bin"
     //         ],
     //         description: "Select a Cumulus OS image file to install on switches."
     //     )
     // }
    environment {
        // GIT URL
        // GIT_URL="git@gitlab.com:nvidia-networking/ethernet_ps/accounts/rakuten-jenkins-pipelines.git"

        // Credential ID needs to be updated accordingly for different Jenkins env.
        // GIT_CREDENTIALS = "d9f16fc1-1954-499d-9a32-f10bf66ac968"

        PRI_SW_FOLDER = "${WORKSPACE}/BUILD_${BUILD_ID}/${params.Primary_Switch}/"
        SEC_SW_FOLDER = "${WORKSPACE}/BUILD_${BUILD_ID}/${params.Secondary_Switch}/"

        PRI_SW_PRE_UPG_FOLDER = "${PRI_SW_FOLDER}" + "pre_upgrade_folder/"
        SEC_SW_PRE_UPG_FOLDER = "${SEC_SW_FOLDER}" + "pre_upgrade_folder/"

        PRI_SW_PRE_UPG_SNAPSHOT = "${PRI_SW_PRE_UPG_FOLDER}" + "snapshot.json"
        SEC_SW_PRE_UPG_SNAPSHOT = "${SEC_SW_PRE_UPG_FOLDER}" + "snapshot.json"

        PRI_SW_POST_UPG_FOLDER = "${PRI_SW_FOLDER}" + "post_upgrade_folder/"
        SEC_SW_POST_UPG_FOLDER = "${SEC_SW_FOLDER}" + "post_upgrade_folder/"

        PRI_SW_POST_UPG_SNAPSHOT = "${PRI_SW_POST_UPG_FOLDER}" + "snapshot.json"
        SEC_SW_POST_UPG_SNAPSHOT = "${SEC_SW_POST_UPG_FOLDER}" + "snapshot.json"

        PRI_SW_UPG_RESULTS = "${PRI_SW_FOLDER}" + "post_upg_validation_results.json"
        SEC_SW_UPG_RESULTS = "${SEC_SW_FOLDER}" + "post_upg_validation_results.json"

        BUILD_RESULTS = "${WORKSPACE}/BUILD_${BUILD_ID}/" + "build_results.json"

        CUMULUS_IMAGE_PATH = "/data/" + "${params.Cumulus_OS}"
    }
    stages {
        // stage("SCM Checkout") {
        //     steps {
        //         dir("${WORKSPACE}") {
        //             git branch: "master", credentialsId: "${GIT_CREDENTIALS}", url: "${GIT_URL}"
        //         }
        //     }
        // }
        stage("Pre-Upgrade Stages: Collect Switch Info") {
            // input {
            //     message "Ready to continue?"
            //     ok "Yessss"
            // }
            steps {
			    echo 'pre-upgrade'
                // sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/collect_switch_info.yaml -e hostlist=${params.Primary_Switch}   -e output_dir=${PRI_SW_PRE_UPG_FOLDER}"
                // sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/collect_switch_info.yaml -e hostlist=${params.Secondary_Switch} -e output_dir=${SEC_SW_PRE_UPG_FOLDER}"
            }
        }
        stage("Pre-Upgrade Stages: Create Snaptshots") {
            // input {
            //     message "Ready to continue?"
            //     ok "Yes"
            // }
            steps {
			    echo 'pre-Snaptshots'
                // sh "/usr/bin/python3 ${WORKSPACE}/scripts/create_sw_snapshots.py -i ${PRI_SW_PRE_UPG_FOLDER} -o ${PRI_SW_PRE_UPG_SNAPSHOT}"
                // sh "/usr/bin/python3 ${WORKSPACE}/scripts/create_sw_snapshots.py -i ${SEC_SW_PRE_UPG_FOLDER} -o ${SEC_SW_PRE_UPG_SNAPSHOT}"
            }
        }
        stage('Pull SDN_Automation GitHub') {
			steps {
            dir('sdnautomation') {
                git branch: "main", credentialsId: 'ddd', url: "https://github.com/songweiaaa/sw.git"
            }
			}
        }
		

		
	
        // stage("Secondary Switch Upgrade Stages: Isolate Secondary SW From Network") {
        //     input {
        //         message "Ready to continue?"
        //         ok "Yes"
        //     }
        //     steps {
        //         sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/vxlan_scenario_reconfig_sw_interfaces.yaml -e hostlist=${params.Secondary_Switch} -e shutdown_all_ports=true"
        //     }
        // }
        // stage("Secondary Switch Upgrade Stages: Install New Image") {
        //     input {
        //         message "Ready to continue?"
        //         ok "Yes"
        //     }
        //     steps {
        //         sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/upgrade_cumulus_os.yaml -e hostlist=${params.Secondary_Switch} -e cumulus_os_image=${params.Cumulus_OS} -e cumulus_os_image_path=${CUMULUS_IMAGE_PATH}"
        //     }
        // }
        // stage("Secondary Switch Upgrade Stages: Restore Secondary SW Configs") {
        //     input {
        //         message "Ready to continue?"
        //         ok "Yes"
        //     }
        //     steps {
        //         sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/vxlan_scenario_config_switches.yaml -e hostlist=${params.Secondary_Switch} -e shutdown_all_ports=true"
        //     }
        // }
        // stage("ToR Switchover Stages: Swap MLAG Roles") {
        //     input {
        //         message "Ready to continue?"
        //         ok "Yes"
        //     }
        //     steps {
        //         echo "bring up peerlink"
        //         sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/vxlan_scenario_reconfig_sw_interfaces.yaml -e hostlist=${params.Secondary_Switch} -e shutdown_service_ports=true"
        //         echo "wait for MLAG convergence"
        //         sleep 300
        //         sh "/usr/bin/ansible ${params.Secondary_Switch} -m shell -a 'clagctl priority 1'"
        //         echo " wait for MLAG switchover to complete"
        //         sleep 10
        //     }
        // }
        // stage("ToR Switchover Stages: Isolate Primary SW From Network") {
        //     input {
        //         message "Ready to continue?"
        //         ok "Yes"
        //     }
        //     steps {
        //         sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/vxlan_scenario_reconfig_sw_interfaces.yaml -e hostlist=${params.Primary_Switch} -e shutdown_all_ports=true"
        //     }
        // }
        // stage("ToR Switchover Stages: Add Secondary SW Back To Network") {
        //     input {
        //         message "Ready to continue?"
        //         ok "Yes"
        //     }
        //     steps {
        //         sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/vxlan_scenario_reconfig_sw_interfaces.yaml -e hostlist=${params.Secondary_Switch}"
        //     }
        // }
        // stage("Primary Switch Upgrade Stages: Install New Image") {
        //     input {
        //         message "Ready to continue?"
        //         ok "Yes"
        //     }
        //     steps {
        //         sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/upgrade_cumulus_os.yaml -e hostlist=${params.Primary_Switch} -e cumulus_os_image=${params.Cumulus_OS} -e cumulus_os_image_path=${CUMULUS_IMAGE_PATH}"
        //     }
        // }
        // stage("Primary Switch Upgrade Stages: Restore Primary SW Configs") {
        //     input {
        //         message "Ready to continue?"
        //         ok "Yes"
        //     }
        //     steps {
        //         sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/vxlan_scenario_config_switches.yaml -e hostlist=${params.Primary_Switch} -e shutdown_all_ports=true"
        //     }
        // }
        // stage("Primary Switch Upgrade Stages: Add Primary SW Back to Network") {
        //     input {
        //         message "Ready to continue?"
        //         ok "Yes"
        //     }
        //     steps {
        //         sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/vxlan_scenario_reconfig_sw_interfaces.yaml -e hostlist=${params.Primary_Switch}"
        //         echo "wait for MLAG convergence"
        //         sleep 300
        //     }
        // }
        // stage("Primary Switch Upgrade Stages: Swap MLAG Roles Back") {
        //     input {
        //         message "Ready to continue?"
        //         ok "Yes"
        //     }
        //     steps {
        //         sh "/usr/bin/ansible ${params.Secondary_Switch} -m shell -a 'clagctl priority 32768'"
        //         echo " wait for MLAG switchover to complete"
        //         sleep 10
        //     }
        // }
        // stage("Post-Upgrade Stages: Wait 5m for Network To Become Stable") {
        //     input {
        //         message "Ready to continue?"
        //         ok "Yes"
        //     }
        //     steps {
        //         sleep 300
        //     }
        // }
        // stage("Post-Upgrade Stages: Collect Switch Info") {
        //     input {
        //         message "Ready to continue?"
        //         ok "Yes"
        //     }
        //     steps {
        //         sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/collect_switch_info.yaml -e hostlist=${params.Primary_Switch}   -e output_dir=${PRI_SW_POST_UPG_FOLDER}"
        //         sh "/usr/bin/ansible-playbook ${WORKSPACE}/playbooks/collect_switch_info.yaml -e hostlist=${params.Secondary_Switch} -e output_dir=${SEC_SW_POST_UPG_FOLDER}"
        //     }
        // }
        // stage("Post-Upgrade Stages: Create Snaptshots") {
        //     input {
        //         message "Ready to continue?"
        //         ok "Yes"
        //     }
        //     steps {
        //         sh "/usr/bin/python3 ${WORKSPACE}/scripts/create_sw_snapshots.py -i ${PRI_SW_POST_UPG_FOLDER} -o ${PRI_SW_POST_UPG_SNAPSHOT}"
        //         sh "/usr/bin/python3 ${WORKSPACE}/scripts/create_sw_snapshots.py -i ${SEC_SW_POST_UPG_FOLDER} -o ${SEC_SW_POST_UPG_SNAPSHOT}"
        //     }
        // }
        // stage("Post-Upgrade Stages: Compare Snaptshots") {
        //     input {
        //         message "Ready to continue?"
        //         ok "Yes"
        //     }
        //     steps {
        //         sh "/usr/bin/python3 ${WORKSPACE}/scripts/compare_sw_pair_snapshots.py -t ${PRI_SW_FOLDER} -r ${SEC_SW_FOLDER} -c ${params.Cumulus_OS} -o ${PRI_SW_UPG_RESULTS}"
        //         sh "/usr/bin/python3 ${WORKSPACE}/scripts/compare_sw_pair_snapshots.py -t ${SEC_SW_FOLDER} -r ${PRI_SW_FOLDER} -c ${params.Cumulus_OS} -o ${SEC_SW_UPG_RESULTS}"
        //     }
        // }
        stage("Post-Upgrade Stages: Parse Post-Upgrade Validation Results") {
            // input {
            //     message "Ready to continue?"
            //     ok "Yes"
            // }
            steps {
                echo "${params.Cumulus_OS}"
                echo "${PRI_SW_FOLDER}"
                echo "${SEC_SW_FOLDER}"
                echo "${CUMULUS_IMAGE_PATH}"
                // sh "/usr/bin/python3 ${WORKSPACE}/scripts/check_sw_pair_upg_results.py -p ${PRI_SW_UPG_RESULTS} -s ${SEC_SW_UPG_RESULTS}"
            }
        }
    }
}

.. code-block:: json

 {
  "status": {
    "state": "ACTIVE",
    "message_list": [],
    "description": "Accessibility:\n* Using mongodb command line `sudo mongo --host [IP_ADDRESS] --port 27017`\n",
    "resources": {
      "service_definition_list": [
        {
          "singleton": false,
          "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe",
          "action_list": [
            {
              "description": "System action for creating an application",
              "type": "system",
              "uuid": "119672ca-1a58-4d98-a256-c12496d50422",
              "state": "ACTIVE",
              "critical": false,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "afc1e408_dag",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "3a40620d-9a38-41e0-b506-6f2af34e7cc2"
                  }
                ],
                "description": "",
                "name": "b9357b85_runbook",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "afc1e408_dag",
                  "uuid": "3a40620d-9a38-41e0-b506-6f2af34e7cc2"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "c05fe841-6938-42a8-88c9-7f33e6d7d9f4"
              },
              "message_list": [],
              "name": "action_create"
            },
            {
              "description": "System action for deleting an application. Deletes physical machines as well",
              "type": "system",
              "uuid": "d3342f4a-3b58-4fa5-a928-44d9a4e94899",
              "state": "ACTIVE",
              "critical": false,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "b0b70621_dag",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "faaf629e-ae7c-4b85-ad9d-f68b10cb19dd"
                  }
                ],
                "description": "",
                "name": "10af4ae1_runbook",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "b0b70621_dag",
                  "uuid": "faaf629e-ae7c-4b85-ad9d-f68b10cb19dd"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "c6f4df33-07f7-4a79-b87b-7fe3ec243b48"
              },
              "message_list": [],
              "name": "action_delete"
            },
            {
              "description": "System action for starting an application",
              "type": "system",
              "uuid": "ad28b31a-c247-426b-aae7-2dde327de3a8",
              "state": "ACTIVE",
              "critical": false,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "ConfigureShards",
                        "uuid": "6be99dac-38dd-4b10-b005-b8e437917f2c"
                      }
                    ],
                    "name": "7f27b711_dag",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "7d20b614-e136-4837-9997-5efff2c9647d"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "ConfigureShards",
                    "state": "ACTIVE",
                    "attrs": {
                      "exit_status": [],
                      "script": "#!/bin/bash\nNODES_PER_REPLICA_SET=\"@@{NODES_PER_REPLICA_SET}@@\"\nHOST_IP=\"@@{address}@@\"\nreplica_ips=$(echo @@{Mongo_ReplicaSet.address}@@ | sed 's/^,//' | sed 's/,$//' | tr \",\" \"\\n\")\n\n\n### Setting up shards\nreplica_cnt=0\nfor replica in $replica_ips\ndo\n  tmp=$((${replica_cnt}+${NODES_PER_REPLICA_SET}))\n  replicaSetName=\"dataReplSet$((${tmp}/${NODES_PER_REPLICA_SET}))\"\n  sudo mongo --host ${HOST_IP} --port 27017 --eval \"sh.addShard( \\\"$replicaSetName/$replica:27017\\\" )\"\n  replica_cnt=$(($replica_cnt + 1))\ndone\n\n",
                      "script_type": "sh",
                      "type": "",
                      "command_line_args": "",
                      "login_credential_local_reference": {
                        "kind": "app_credential",
                        "name": "CENTOS",
                        "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                      }
                    },
                    "timeout_secs": "",
                    "type": "EXEC",
                    "variable_list": [],
                    "uuid": "6be99dac-38dd-4b10-b005-b8e437917f2c"
                  }
                ],
                "description": "",
                "name": "d9638df2_runbook",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "7f27b711_dag",
                  "uuid": "7d20b614-e136-4837-9997-5efff2c9647d"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "5c819c37-f08f-442e-af24-e72806300a94"
              },
              "message_list": [],
              "name": "action_start"
            },
            {
              "description": "System action for stopping an application",
              "type": "system",
              "uuid": "9ba5414e-c372-41ab-a6f6-036755c368ae",
              "state": "ACTIVE",
              "critical": false,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "418ea855_dag",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "20f62f80-d6b4-4027-9efb-adbb685befed"
                  }
                ],
                "description": "",
                "name": "507a01b5_runbook",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "418ea855_dag",
                  "uuid": "20f62f80-d6b4-4027-9efb-adbb685befed"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "3496374f-c76e-4ba7-bc72-53e1a0475fe3"
              },
              "message_list": [],
              "name": "action_stop"
            },
            {
              "description": "System action for restarting an application",
              "type": "system",
              "uuid": "007596f9-31ea-4097-b86c-a17d00a0b1bc",
              "state": "ACTIVE",
              "critical": false,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "b5c5a93d_dag",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "67ef7dcb-a531-4884-9dc3-e20c5a81a001"
                  }
                ],
                "description": "",
                "name": "c46e4637_runbook",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "b5c5a93d_dag",
                  "uuid": "67ef7dcb-a531-4884-9dc3-e20c5a81a001"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "d55ef03b-5168-4627-b43b-5156ff0b28d8"
              },
              "message_list": [],
              "name": "action_restart"
            },
            {
              "description": "System action for deleting an application. Does not delete physical machines",
              "type": "system",
              "uuid": "c248028d-16fc-4923-be43-741e8446681d",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Service_Element_Delete_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "5a017477-2c89-4089-8304-1caa1b1def5d"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "4247a010-5470-4a17-ac7b-cee25f3b888e"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Service_Element_Delete_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "SOFT_DELETE_ELEMENT",
                    "variable_list": [],
                    "uuid": "5a017477-2c89-4089-8304-1caa1b1def5d"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                  "uuid": "4247a010-5470-4a17-ac7b-cee25f3b888e"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "67c7551e-6768-46db-bde8-e28af739c02f"
              },
              "message_list": [],
              "name": "action_soft_delete"
            }
          ],
          "depends_on_list": [],
          "name": "Mongo_Router",
          "state": "ACTIVE",
          "port_list": [],
          "editables": {},
          "tier": "",
          "message_list": [],
          "variable_list": [],
          "description": ""
        },
        {
          "singleton": false,
          "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5",
          "action_list": [
            {
              "description": "System action for creating an application",
              "type": "system",
              "uuid": "86b125d1-7030-47eb-bc43-7d2944783e30",
              "state": "ACTIVE",
              "critical": false,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "8c950d87_dag",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "c9050c5f-7caf-4827-95bc-6457ca81401f"
                  }
                ],
                "description": "",
                "name": "e8608054_runbook",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "8c950d87_dag",
                  "uuid": "c9050c5f-7caf-4827-95bc-6457ca81401f"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "5dd0f77d-1c9e-4bbb-9d61-50c834876150"
              },
              "message_list": [],
              "name": "action_create"
            },
            {
              "description": "System action for deleting an application. Deletes physical machines as well",
              "type": "system",
              "uuid": "c76a02be-093c-41ea-a624-a94082239399",
              "state": "ACTIVE",
              "critical": false,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "3eb93418_dag",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "c2d42876-9ac9-4bde-8797-2c20fdaf5fee"
                  }
                ],
                "description": "",
                "name": "fd485dc4_runbook",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "3eb93418_dag",
                  "uuid": "c2d42876-9ac9-4bde-8797-2c20fdaf5fee"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "bd261c01-2c76-46fd-a615-846ed9b187b1"
              },
              "message_list": [],
              "name": "action_delete"
            },
            {
              "description": "System action for starting an application",
              "type": "system",
              "uuid": "ba8861fb-9116-42e6-a978-c7dce368c564",
              "state": "ACTIVE",
              "critical": false,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "8488e56f_dag",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "b67c9e91-ff22-4083-8cea-e0b292cbb754"
                  }
                ],
                "description": "",
                "name": "d04295dc_runbook",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "8488e56f_dag",
                  "uuid": "b67c9e91-ff22-4083-8cea-e0b292cbb754"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "b4ac0257-a074-4e37-9f8b-9df8a30127f1"
              },
              "message_list": [],
              "name": "action_start"
            },
            {
              "description": "System action for stopping an application",
              "type": "system",
              "uuid": "e6701689-6977-4ab4-876a-08701bd0d581",
              "state": "ACTIVE",
              "critical": false,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "1000364d_dag",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "f4db7e8c-d5e6-4e0f-b085-0a7e715806b4"
                  }
                ],
                "description": "",
                "name": "c93943b8_runbook",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "1000364d_dag",
                  "uuid": "f4db7e8c-d5e6-4e0f-b085-0a7e715806b4"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "c2ba6f77-45ff-4d0c-80bc-6a13a88bac4e"
              },
              "message_list": [],
              "name": "action_stop"
            },
            {
              "description": "System action for restarting an application",
              "type": "system",
              "uuid": "696f5b8f-d0f0-4a2d-bc04-58b509a332a6",
              "state": "ACTIVE",
              "critical": false,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "646ec79e_dag",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "99173eb6-a1aa-4b73-9f9e-fdcd40544cef"
                  }
                ],
                "description": "",
                "name": "529ff634_runbook",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "646ec79e_dag",
                  "uuid": "99173eb6-a1aa-4b73-9f9e-fdcd40544cef"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "bdc9c3d4-6e97-43c7-9443-5543a5230a9e"
              },
              "message_list": [],
              "name": "action_restart"
            },
            {
              "description": "System action for deleting an application. Does not delete physical machines",
              "type": "system",
              "uuid": "24f16990-5118-44a3-92fe-4d7d7c347976",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Service_Element_Delete_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "ff30edeb-bc8d-49ce-807d-d4f501ef99e2"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "9bfc07da-531d-400b-aa66-43d55d85d254"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Service_Element_Delete_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "SOFT_DELETE_ELEMENT",
                    "variable_list": [],
                    "uuid": "ff30edeb-bc8d-49ce-807d-d4f501ef99e2"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                  "uuid": "9bfc07da-531d-400b-aa66-43d55d85d254"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "ae786a91-e961-47bb-afe7-05a0b7244131"
              },
              "message_list": [],
              "name": "action_soft_delete"
            }
          ],
          "depends_on_list": [],
          "name": "Mongo_ConfigSet",
          "state": "ACTIVE",
          "port_list": [],
          "editables": {},
          "tier": "",
          "message_list": [],
          "variable_list": [],
          "description": ""
        },
        {
          "singleton": false,
          "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7",
          "action_list": [
            {
              "description": "System action for creating an application",
              "type": "system",
              "uuid": "a8052bb9-59c2-4300-9a1c-eca32b1c613e",
              "state": "ACTIVE",
              "critical": false,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "3f585e04_dag",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "547daee0-a213-402b-894f-7bbc0c51b533"
                  }
                ],
                "description": "",
                "name": "e984fac1_runbook",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "3f585e04_dag",
                  "uuid": "547daee0-a213-402b-894f-7bbc0c51b533"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "c4094263-6a43-46f6-9b5e-335b65860978"
              },
              "message_list": [],
              "name": "action_create"
            },
            {
              "description": "System action for deleting an application. Deletes physical machines as well",
              "type": "system",
              "uuid": "0baca3a5-0100-4afa-98a4-f281ed4c9548",
              "state": "ACTIVE",
              "critical": false,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "2e34dabb_dag",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "cc3dd0d3-eb8d-4360-9447-fe31716fccc0"
                  }
                ],
                "description": "",
                "name": "96df4d94_runbook",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "2e34dabb_dag",
                  "uuid": "cc3dd0d3-eb8d-4360-9447-fe31716fccc0"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "8d242223-afda-4c13-b35f-b9dff6717e5e"
              },
              "message_list": [],
              "name": "action_delete"
            },
            {
              "description": "System action for starting an application",
              "type": "system",
              "uuid": "c0c5d7a5-ef75-44e6-afab-f47085656b11",
              "state": "ACTIVE",
              "critical": false,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "e00c41e9_dag",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "3e9c45b1-5e16-4331-8155-777114067f41"
                  }
                ],
                "description": "",
                "name": "6286b364_runbook",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "e00c41e9_dag",
                  "uuid": "3e9c45b1-5e16-4331-8155-777114067f41"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "b414ddcc-2221-4078-bac1-8c739a480a49"
              },
              "message_list": [],
              "name": "action_start"
            },
            {
              "description": "System action for stopping an application",
              "type": "system",
              "uuid": "5ceedbe9-0883-4915-9793-bcddd876c4fd",
              "state": "ACTIVE",
              "critical": false,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "01c64f18_dag",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "f859f692-52c4-44bf-bee1-4f0c626b70c2"
                  }
                ],
                "description": "",
                "name": "ee3949e3_runbook",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "01c64f18_dag",
                  "uuid": "f859f692-52c4-44bf-bee1-4f0c626b70c2"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "706d1aab-530f-452d-8b33-15bfbdb2fdd7"
              },
              "message_list": [],
              "name": "action_stop"
            },
            {
              "description": "System action for restarting an application",
              "type": "system",
              "uuid": "a264145a-9f10-4e18-bb70-417a6aa500b0",
              "state": "ACTIVE",
              "critical": false,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "52c456b0_dag",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "3e14dc35-8525-492b-a549-f27da36cf084"
                  }
                ],
                "description": "",
                "name": "0ebe0bd0_runbook",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "52c456b0_dag",
                  "uuid": "3e14dc35-8525-492b-a549-f27da36cf084"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "7b5204f6-5443-422c-ae0e-d08476d9d37a"
              },
              "message_list": [],
              "name": "action_restart"
            },
            {
              "description": "System action for deleting an application. Does not delete physical machines",
              "type": "system",
              "uuid": "0fddb950-ae61-443e-8138-a806505af0e6",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Service_Element_Delete_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "d07cb93c-51cb-4731-90fa-53650637d287"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "cbfa464b-a8c4-4513-ba7e-dff5ac8bb95d"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Service_Element_Delete_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "SOFT_DELETE_ELEMENT",
                    "variable_list": [],
                    "uuid": "d07cb93c-51cb-4731-90fa-53650637d287"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                  "uuid": "cbfa464b-a8c4-4513-ba7e-dff5ac8bb95d"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "c0f5e1e4-bdb8-4981-baa5-7f01c4a753ad"
              },
              "message_list": [],
              "name": "action_soft_delete"
            }
          ],
          "depends_on_list": [],
          "name": "Mongo_ReplicaSet",
          "state": "ACTIVE",
          "port_list": [],
          "editables": {},
          "tier": "",
          "message_list": [],
          "variable_list": [],
          "description": ""
        }
      ],
      "substrate_definition_list": [
        {
          "description": "",
          "action_list": [
            {
              "description": "System action for creating an application",
              "type": "system",
              "uuid": "ac31ed8f-14fa-43f7-af26-bbec8f794975",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Nutanix_Provision_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "fb313719-877b-4d84-95c5-dde7807cbc4c"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__check_login_for_AHVRouter_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "6c298915-2b0a-4440-bf69-f2aa10f1db30"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Nutanix_Provision_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "fb313719-877b-4d84-95c5-dde7807cbc4c"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "25184c51-405c-4182-9e30-568a2a204e64",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__check_login_for_AHVRouter_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "6c298915-2b0a-4440-bf69-f2aa10f1db30"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "ac483d8c-11d6-42e1-bf7a-7ecc97d76648"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Nutanix_Provision_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "name": "MongoRouterVM",
                      "availability_zone_reference": null,
                      "backup_policy": null,
                      "type": "",
                      "cluster_reference": null,
                      "resources": {
                        "nic_list": [
                          {
                            "nic_type": "NORMAL_NIC",
                            "ip_endpoint_list": [],
                            "network_function_chain_reference": null,
                            "network_function_nic_type": "INGRESS",
                            "mac_address": "",
                            "subnet_reference": {
                              "kind": "subnet",
                              "type": "",
                              "name": "",
                              "uuid": "a6bb0e0b-fcb0-424c-95b3-5c700d11bdb8"
                            },
                            "type": ""
                          }
                        ],
                        "power_state": "ON",
                        "guest_tools": null,
                        "num_vcpus_per_socket": 1,
                        "num_sockets": 2,
                        "gpu_list": [],
                        "memory_size_mib": 4096,
                        "parent_reference": null,
                        "hardware_clock_timezone": "",
                        "guest_customization": null,
                        "type": "",
                        "boot_config": {
                          "boot_device": {
                            "type": "",
                            "disk_address": {
                              "type": "",
                              "device_index": 0,
                              "adapter_type": "SCSI"
                            }
                          },
                          "type": "",
                          "mac_address": ""
                        },
                        "disk_list": [
                          {
                            "data_source_reference": {
                              "kind": "image",
                              "type": "",
                              "name": "CentOS Server v7",
                              "uuid": "cb241054-89c4-40c4-8872-dcdbf7fa104d"
                            },
                            "type": "",
                            "disk_size_mib": 0,
                            "device_properties": {
                              "type": "",
                              "disk_address": {
                                "type": "",
                                "device_index": 0,
                                "adapter_type": "SCSI"
                              },
                              "device_type": "DISK"
                            }
                          }
                        ]
                      }
                    },
                    "timeout_secs": "",
                    "type": "PROVISION_NUTANIX",
                    "variable_list": [],
                    "uuid": "fb313719-877b-4d84-95c5-dde7807cbc4c"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__check_login_for_AHVRouter_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CHECK_LOGIN",
                      "timeout": "60",
                      "address": "@@{platform.status.resources.nic_list[0].ip_endpoint_list[0].ip}@@"
                    },
                    "timeout_secs": "",
                    "type": "CHECK_LOGIN",
                    "variable_list": [],
                    "uuid": "6c298915-2b0a-4440-bf69-f2aa10f1db30"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                  "uuid": "ac483d8c-11d6-42e1-bf7a-7ecc97d76648"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "deaf522d-a578-48f2-b281-e446ee769188"
              },
              "message_list": [],
              "name": "action_create"
            },
            {
              "description": "System action for powering on an application",
              "type": "system",
              "uuid": "c12b39e8-b89a-4545-90b1-412129bd8f62",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Nutanix_Operation_PowerOn_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "ddb39a24-f361-429c-b0e3-1db7a493769f"
                      },
                      {
                        "kind": "app_task",
                        "name": "check_login_for_AHVRouter_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "7b9777cb-6075-476d-934a-fd6fbad3fa41"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Nutanix_Operation_PowerOn_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "ddb39a24-f361-429c-b0e3-1db7a493769f"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "d8d5f431-229e-4149-81e0-dd094eef9c4d",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "check_login_for_AHVRouter_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "7b9777cb-6075-476d-934a-fd6fbad3fa41"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "f2a802b8-24d7-4dbb-8d0f-866f051d74df"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Nutanix_Operation_PowerOn_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "GENERIC_OPERATION"
                    },
                    "timeout_secs": "",
                    "type": "VMOPERATION_NUTANIX",
                    "variable_list": [],
                    "uuid": "ddb39a24-f361-429c-b0e3-1db7a493769f"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "check_login_for_AHVRouter_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CHECK_LOGIN",
                      "timeout": "60",
                      "address": "@@{platform.status.resources.nic_list[0].ip_endpoint_list[0].ip}@@"
                    },
                    "timeout_secs": "",
                    "type": "CHECK_LOGIN",
                    "variable_list": [],
                    "uuid": "7b9777cb-6075-476d-934a-fd6fbad3fa41"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                  "uuid": "f2a802b8-24d7-4dbb-8d0f-866f051d74df"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "5cdcc37b-17ca-49d7-a065-d0e8824c9176"
              },
              "message_list": [],
              "name": "action_poweron"
            },
            {
              "description": "System action for suspending an application",
              "type": "system",
              "uuid": "f762ef59-f3e8-463a-9931-0148058fad85",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__DAG_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "e80272cb-bb3e-4ca5-a790-71e3929c4ccf"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                  "uuid": "e80272cb-bb3e-4ca5-a790-71e3929c4ccf"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "4b9bc264-dd36-4f3e-87bd-b08dbcee1640"
              },
              "message_list": [],
              "name": "action_suspend"
            },
            {
              "description": "System action for modifying an application",
              "type": "system",
              "uuid": "b7ccd613-8810-4714-ade4-414716aef9c5",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__DAG_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "75ead986-6a2f-4c12-bcd6-1667ffdc3372"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                  "uuid": "75ead986-6a2f-4c12-bcd6-1667ffdc3372"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "1abf86f4-f871-4f8f-9291-d2980fc0550c"
              },
              "message_list": [],
              "name": "action_modify"
            },
            {
              "description": "System action for powering off an application",
              "type": "system",
              "uuid": "44e16e45-0d23-44d8-ad9d-a7dd976d658e",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Nutanix_Operation_PowerOff_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "e0a4b9d8-f67e-44bb-876c-ce570cfce39c"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "f2199f4e-2e29-4057-8ecb-657aa1787293"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Nutanix_Operation_PowerOff_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "GENERIC_OPERATION"
                    },
                    "timeout_secs": "",
                    "type": "VMOPERATION_NUTANIX",
                    "variable_list": [],
                    "uuid": "e0a4b9d8-f67e-44bb-876c-ce570cfce39c"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                  "uuid": "f2199f4e-2e29-4057-8ecb-657aa1787293"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "b1596526-1c86-439a-bc55-2c21dc7bd5f4"
              },
              "message_list": [],
              "name": "action_poweroff"
            },
            {
              "description": "System action for deleting an application. Deletes physical machines as well",
              "type": "system",
              "uuid": "74a909e3-af6b-4255-bb0b-92380d225084",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Nutanix_Delete_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "8313facb-5ece-4296-ab0b-53f0481b1066"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "211f77f2-f871-4379-b8e6-c9a70baa08fa"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Nutanix_Delete_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DELETE_NUTANIX",
                    "variable_list": [],
                    "uuid": "8313facb-5ece-4296-ab0b-53f0481b1066"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                  "uuid": "211f77f2-f871-4379-b8e6-c9a70baa08fa"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "39f030aa-de7a-4bd0-b4bc-8ca45308b4cf"
              },
              "message_list": [],
              "name": "action_delete"
            },
            {
              "description": "System action for deleting an application. Does not delete physical machines",
              "type": "system",
              "uuid": "66a53409-7dd1-48c4-873d-02f5966ee11a",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Substrate_Element_Delete_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "661846e6-a8d0-4e19-b52f-da6360105d6d"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "b0d49c9b-884e-4d84-a8db-b4012a0287ad"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Substrate_Element_Delete_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "SOFT_DELETE_ELEMENT",
                    "variable_list": [],
                    "uuid": "661846e6-a8d0-4e19-b52f-da6360105d6d"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                  "uuid": "b0d49c9b-884e-4d84-a8db-b4012a0287ad"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "50a06689-a733-4df5-910b-847260cca798"
              },
              "message_list": [],
              "name": "action_soft_delete"
            }
          ],
          "type": "AHV_VM",
          "name": "AHVRouter",
          "state": "ACTIVE",
          "readiness_probe": {
            "connection_type": "SSH",
            "disable_readiness_probe": false,
            "timeout_secs": "60",
            "address": "@@{platform.status.resources.nic_list[0].ip_endpoint_list[0].ip}@@",
            "connection_port": 22,
            "login_credential_local_reference": {
              "kind": "app_credential",
              "name": "CENTOS",
              "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
            }
          },
          "editables": {
            "readiness_probe": {
              "connection_port": true,
              "timeout_secs": true
            },
            "create_spec": {
              "name": true,
              "resources": {
                "nic_list": {
                  "0": {
                    "subnet_reference": true
                  }
                },
                "num_vcpus_per_socket": true,
                "num_sockets": true,
                "memory_size_mib": true,
                "guest_customization": true,
                "disk_list": {
                  "0": {
                    "device_properties": {
                      "disk_address": {}
                    }
                  }
                }
              }
            }
          },
          "os_type": "Linux",
          "message_list": [],
          "create_spec": {
            "name": "MongoRouterVM",
            "availability_zone_reference": null,
            "backup_policy": null,
            "type": "",
            "cluster_reference": null,
            "resources": {
              "nic_list": [
                {
                  "nic_type": "NORMAL_NIC",
                  "ip_endpoint_list": [],
                  "network_function_chain_reference": null,
                  "network_function_nic_type": "INGRESS",
                  "mac_address": "",
                  "subnet_reference": {
                    "kind": "subnet",
                    "type": "",
                    "name": "",
                    "uuid": "a6bb0e0b-fcb0-424c-95b3-5c700d11bdb8"
                  },
                  "type": ""
                }
              ],
              "power_state": "ON",
              "guest_tools": null,
              "num_vcpus_per_socket": 1,
              "num_sockets": 2,
              "gpu_list": [],
              "memory_size_mib": 4096,
              "parent_reference": null,
              "hardware_clock_timezone": "",
              "guest_customization": null,
              "type": "",
              "boot_config": {
                "boot_device": {
                  "type": "",
                  "disk_address": {
                    "type": "",
                    "device_index": 0,
                    "adapter_type": "SCSI"
                  }
                },
                "type": "",
                "mac_address": ""
              },
              "disk_list": [
                {
                  "data_source_reference": {
                    "kind": "image",
                    "type": "",
                    "name": "CentOS Server v7",
                    "uuid": "cb241054-89c4-40c4-8872-dcdbf7fa104d"
                  },
                  "type": "",
                  "disk_size_mib": 0,
                  "device_properties": {
                    "type": "",
                    "disk_address": {
                      "type": "",
                      "device_index": 0,
                      "adapter_type": "SCSI"
                    },
                    "device_type": "DISK"
                  }
                }
              ]
            }
          },
          "variable_list": [],
          "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
        },
        {
          "description": "",
          "action_list": [
            {
              "description": "System action for creating an application",
              "type": "system",
              "uuid": "7a40f276-5a48-4dd4-831a-8dde8ca477d9",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Nutanix_Provision_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "38887571-5ee6-45e4-9386-a4bdad8b5372"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__check_login_for_AHVConfigSet_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "4ef2320e-9995-4765-b609-41aab6adfe40"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Nutanix_Provision_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "38887571-5ee6-45e4-9386-a4bdad8b5372"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "fe999a6d-f2af-4cda-99ba-47ff5adbc5c3",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__check_login_for_AHVConfigSet_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "4ef2320e-9995-4765-b609-41aab6adfe40"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "d29f56ee-c855-4db1-af5e-a8cf84b3d8c5"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Nutanix_Provision_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "name": "MongoConfigSetVM",
                      "availability_zone_reference": null,
                      "backup_policy": null,
                      "type": "",
                      "cluster_reference": null,
                      "resources": {
                        "nic_list": [
                          {
                            "nic_type": "NORMAL_NIC",
                            "ip_endpoint_list": [],
                            "network_function_chain_reference": null,
                            "network_function_nic_type": "INGRESS",
                            "mac_address": "",
                            "subnet_reference": {
                              "kind": "subnet",
                              "type": "",
                              "name": "",
                              "uuid": "a6bb0e0b-fcb0-424c-95b3-5c700d11bdb8"
                            },
                            "type": ""
                          }
                        ],
                        "power_state": "ON",
                        "guest_tools": null,
                        "num_vcpus_per_socket": 1,
                        "num_sockets": 2,
                        "gpu_list": [],
                        "memory_size_mib": 4096,
                        "parent_reference": null,
                        "hardware_clock_timezone": "",
                        "guest_customization": null,
                        "type": "",
                        "boot_config": null,
                        "disk_list": [
                          {
                            "data_source_reference": {
                              "kind": "image",
                              "type": "",
                              "name": "CentOS Server v7",
                              "uuid": "cb241054-89c4-40c4-8872-dcdbf7fa104d"
                            },
                            "type": "",
                            "disk_size_mib": 0,
                            "device_properties": {
                              "type": "",
                              "disk_address": {
                                "type": "",
                                "device_index": 0,
                                "adapter_type": "SCSI"
                              },
                              "device_type": "DISK"
                            }
                          }
                        ]
                      }
                    },
                    "timeout_secs": "",
                    "type": "PROVISION_NUTANIX",
                    "variable_list": [],
                    "uuid": "38887571-5ee6-45e4-9386-a4bdad8b5372"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__check_login_for_AHVConfigSet_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CHECK_LOGIN",
                      "timeout": "60",
                      "address": "@@{platform.status.resources.nic_list[0].ip_endpoint_list[0].ip}@@"
                    },
                    "timeout_secs": "",
                    "type": "CHECK_LOGIN",
                    "variable_list": [],
                    "uuid": "4ef2320e-9995-4765-b609-41aab6adfe40"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                  "uuid": "d29f56ee-c855-4db1-af5e-a8cf84b3d8c5"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "16c52300-6568-4f9d-8abf-7fad590a18ac"
              },
              "message_list": [],
              "name": "action_create"
            },
            {
              "description": "System action for powering on an application",
              "type": "system",
              "uuid": "471ef48a-2453-4cd3-af05-f58186f1b903",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Nutanix_Operation_PowerOn_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "2f81d072-b946-4694-a379-ce664388b465"
                      },
                      {
                        "kind": "app_task",
                        "name": "check_login_for_AHVConfigSet_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "dd53034e-5796-4e72-af5a-bb004feab1a0"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Nutanix_Operation_PowerOn_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "2f81d072-b946-4694-a379-ce664388b465"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "8d4c6549-4741-4408-bb66-08033e637292",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "check_login_for_AHVConfigSet_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "dd53034e-5796-4e72-af5a-bb004feab1a0"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "1f523ee6-9369-4959-8dca-a95d10ff9d75"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Nutanix_Operation_PowerOn_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "GENERIC_OPERATION"
                    },
                    "timeout_secs": "",
                    "type": "VMOPERATION_NUTANIX",
                    "variable_list": [],
                    "uuid": "2f81d072-b946-4694-a379-ce664388b465"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "check_login_for_AHVConfigSet_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CHECK_LOGIN",
                      "timeout": "60",
                      "address": "@@{platform.status.resources.nic_list[0].ip_endpoint_list[0].ip}@@"
                    },
                    "timeout_secs": "",
                    "type": "CHECK_LOGIN",
                    "variable_list": [],
                    "uuid": "dd53034e-5796-4e72-af5a-bb004feab1a0"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                  "uuid": "1f523ee6-9369-4959-8dca-a95d10ff9d75"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "3cf91285-df41-4ae8-9a82-d1d1fae9de0e"
              },
              "message_list": [],
              "name": "action_poweron"
            },
            {
              "description": "System action for suspending an application",
              "type": "system",
              "uuid": "f6d30473-98e8-4f37-a5e9-bb5813d71dfd",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__DAG_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "bccb92a1-2722-4629-aabc-0ef5d842dda6"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                  "uuid": "bccb92a1-2722-4629-aabc-0ef5d842dda6"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "5817576b-180e-4a4f-8d5d-4967bf9c78ea"
              },
              "message_list": [],
              "name": "action_suspend"
            },
            {
              "description": "System action for modifying an application",
              "type": "system",
              "uuid": "94ae0866-191c-4e15-bd98-75954470728a",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__DAG_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "55b2b16c-7c09-4f45-84fe-eb651a61fd56"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                  "uuid": "55b2b16c-7c09-4f45-84fe-eb651a61fd56"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "87d8ad4b-368e-4c8a-a1c2-45bf0b0fa80e"
              },
              "message_list": [],
              "name": "action_modify"
            },
            {
              "description": "System action for powering off an application",
              "type": "system",
              "uuid": "d6bca085-fc25-486f-acab-41e908b281d9",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Nutanix_Operation_PowerOff_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "9e81ed9b-5d62-4a3c-a18f-cc868ac2215d"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "7ae1c54f-5830-4559-82d5-0fe65e9a5055"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Nutanix_Operation_PowerOff_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "GENERIC_OPERATION"
                    },
                    "timeout_secs": "",
                    "type": "VMOPERATION_NUTANIX",
                    "variable_list": [],
                    "uuid": "9e81ed9b-5d62-4a3c-a18f-cc868ac2215d"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                  "uuid": "7ae1c54f-5830-4559-82d5-0fe65e9a5055"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "b4df5ceb-d327-4062-9d7b-39e0a1b5981a"
              },
              "message_list": [],
              "name": "action_poweroff"
            },
            {
              "description": "System action for deleting an application. Deletes physical machines as well",
              "type": "system",
              "uuid": "fcf85d84-1e3a-4fc5-bf66-324912c1cdb5",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Nutanix_Delete_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "1d98fc51-52e1-4a73-92ff-4fda81cb808b"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "1a375819-d320-45d7-8fac-9965255fa4b0"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Nutanix_Delete_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DELETE_NUTANIX",
                    "variable_list": [],
                    "uuid": "1d98fc51-52e1-4a73-92ff-4fda81cb808b"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                  "uuid": "1a375819-d320-45d7-8fac-9965255fa4b0"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "4d2b8fca-dccf-4d06-93b9-eb6abbaa71de"
              },
              "message_list": [],
              "name": "action_delete"
            },
            {
              "description": "System action for deleting an application. Does not delete physical machines",
              "type": "system",
              "uuid": "910fa960-efa0-4a9e-bb59-a685ee4386a5",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Substrate_Element_Delete_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "f3b10edb-7bd1-4522-a509-0aaeac23dc1b"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "6071953d-3210-428c-a38e-14bf2e6d0852"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Substrate_Element_Delete_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "SOFT_DELETE_ELEMENT",
                    "variable_list": [],
                    "uuid": "f3b10edb-7bd1-4522-a509-0aaeac23dc1b"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                  "uuid": "6071953d-3210-428c-a38e-14bf2e6d0852"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "4bfd4ceb-6310-4fc9-b694-3568aff778bc"
              },
              "message_list": [],
              "name": "action_soft_delete"
            }
          ],
          "type": "AHV_VM",
          "name": "AHVConfigSet",
          "state": "ACTIVE",
          "readiness_probe": {
            "connection_type": "SSH",
            "disable_readiness_probe": false,
            "timeout_secs": "60",
            "address": "@@{platform.status.resources.nic_list[0].ip_endpoint_list[0].ip}@@",
            "connection_port": 22,
            "login_credential_local_reference": {
              "kind": "app_credential",
              "name": "CENTOS",
              "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
            }
          },
          "editables": {
            "readiness_probe": {
              "connection_port": true,
              "timeout_secs": true
            },
            "create_spec": {
              "name": true,
              "resources": {
                "nic_list": {
                  "0": {
                    "subnet_reference": true
                  }
                },
                "num_vcpus_per_socket": true,
                "num_sockets": true,
                "memory_size_mib": true,
                "guest_customization": true,
                "disk_list": {
                  "0": {
                    "device_properties": {
                      "disk_address": {}
                    }
                  }
                }
              }
            }
          },
          "os_type": "Linux",
          "message_list": [],
          "create_spec": {
            "name": "MongoConfigSetVM",
            "availability_zone_reference": null,
            "backup_policy": null,
            "type": "",
            "cluster_reference": null,
            "resources": {
              "nic_list": [
                {
                  "nic_type": "NORMAL_NIC",
                  "ip_endpoint_list": [],
                  "network_function_chain_reference": null,
                  "network_function_nic_type": "INGRESS",
                  "mac_address": "",
                  "subnet_reference": {
                    "kind": "subnet",
                    "type": "",
                    "name": "",
                    "uuid": "a6bb0e0b-fcb0-424c-95b3-5c700d11bdb8"
                  },
                  "type": ""
                }
              ],
              "power_state": "ON",
              "guest_tools": null,
              "num_vcpus_per_socket": 1,
              "num_sockets": 2,
              "gpu_list": [],
              "memory_size_mib": 4096,
              "parent_reference": null,
              "hardware_clock_timezone": "",
              "guest_customization": null,
              "type": "",
              "boot_config": null,
              "disk_list": [
                {
                  "data_source_reference": {
                    "kind": "image",
                    "type": "",
                    "name": "CentOS Server v7",
                    "uuid": "cb241054-89c4-40c4-8872-dcdbf7fa104d"
                  },
                  "type": "",
                  "disk_size_mib": 0,
                  "device_properties": {
                    "type": "",
                    "disk_address": {
                      "type": "",
                      "device_index": 0,
                      "adapter_type": "SCSI"
                    },
                    "device_type": "DISK"
                  }
                }
              ]
            }
          },
          "variable_list": [],
          "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
        },
        {
          "description": "",
          "action_list": [
            {
              "description": "System action for creating an application",
              "type": "system",
              "uuid": "bff5485a-9aae-4a2f-941c-5ad76e490caa",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Nutanix_Provision_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "2e9f90d4-003e-4274-bc4d-b31122be1070"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__check_login_for_AHVReplicaSet_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "b486a502-7b29-4c1a-bb9f-5de67b51eb52"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Nutanix_Provision_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "2e9f90d4-003e-4274-bc4d-b31122be1070"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "f02531af-9be0-4d8e-8e5f-e506bade0314",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__check_login_for_AHVReplicaSet_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "b486a502-7b29-4c1a-bb9f-5de67b51eb52"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "e3d3ef8c-60db-4d39-a29c-80c3b840bc82"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Nutanix_Provision_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "name": "MongoReplicaSetVM",
                      "availability_zone_reference": null,
                      "backup_policy": null,
                      "type": "",
                      "cluster_reference": null,
                      "resources": {
                        "nic_list": [
                          {
                            "nic_type": "NORMAL_NIC",
                            "ip_endpoint_list": [],
                            "network_function_chain_reference": null,
                            "network_function_nic_type": "INGRESS",
                            "mac_address": "",
                            "subnet_reference": {
                              "kind": "subnet",
                              "type": "",
                              "name": "",
                              "uuid": "a6bb0e0b-fcb0-424c-95b3-5c700d11bdb8"
                            },
                            "type": ""
                          }
                        ],
                        "power_state": "ON",
                        "guest_tools": null,
                        "num_vcpus_per_socket": 1,
                        "num_sockets": 2,
                        "gpu_list": [],
                        "memory_size_mib": 4096,
                        "parent_reference": null,
                        "hardware_clock_timezone": "",
                        "guest_customization": null,
                        "type": "",
                        "boot_config": {
                          "boot_device": {
                            "type": "",
                            "disk_address": {
                              "type": "",
                              "device_index": 0,
                              "adapter_type": "SCSI"
                            }
                          },
                          "type": "",
                          "mac_address": ""
                        },
                        "disk_list": [
                          {
                            "data_source_reference": {
                              "kind": "image",
                              "type": "",
                              "name": "CentOS Server v7",
                              "uuid": "cb241054-89c4-40c4-8872-dcdbf7fa104d"
                            },
                            "type": "",
                            "disk_size_mib": 0,
                            "device_properties": {
                              "type": "",
                              "disk_address": {
                                "type": "",
                                "device_index": 0,
                                "adapter_type": "SCSI"
                              },
                              "device_type": "DISK"
                            }
                          }
                        ]
                      }
                    },
                    "timeout_secs": "",
                    "type": "PROVISION_NUTANIX",
                    "variable_list": [],
                    "uuid": "2e9f90d4-003e-4274-bc4d-b31122be1070"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__check_login_for_AHVReplicaSet_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CHECK_LOGIN",
                      "timeout": "60",
                      "address": "@@{platform.status.resources.nic_list[0].ip_endpoint_list[0].ip}@@"
                    },
                    "timeout_secs": "",
                    "type": "CHECK_LOGIN",
                    "variable_list": [],
                    "uuid": "b486a502-7b29-4c1a-bb9f-5de67b51eb52"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                  "uuid": "e3d3ef8c-60db-4d39-a29c-80c3b840bc82"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "52b73e8a-27bd-41b3-8a7c-d5822b971228"
              },
              "message_list": [],
              "name": "action_create"
            },
            {
              "description": "System action for powering on an application",
              "type": "system",
              "uuid": "5b878b04-c9ba-4629-95de-6474dcb0826a",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Nutanix_Operation_PowerOn_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "5f247130-aa48-4fec-a24e-577745147556"
                      },
                      {
                        "kind": "app_task",
                        "name": "check_login_for_AHVReplicaSet_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "d1c95f38-1877-47f6-8233-80d11ee6f6c5"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Nutanix_Operation_PowerOn_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "5f247130-aa48-4fec-a24e-577745147556"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "04c1ffbf-14d6-404c-bc56-2119cd537c8d",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "check_login_for_AHVReplicaSet_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "d1c95f38-1877-47f6-8233-80d11ee6f6c5"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "29159663-0a9e-4f97-98e4-01d5f4978896"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Nutanix_Operation_PowerOn_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "GENERIC_OPERATION"
                    },
                    "timeout_secs": "",
                    "type": "VMOPERATION_NUTANIX",
                    "variable_list": [],
                    "uuid": "5f247130-aa48-4fec-a24e-577745147556"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "check_login_for_AHVReplicaSet_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CHECK_LOGIN",
                      "timeout": "60",
                      "address": "@@{platform.status.resources.nic_list[0].ip_endpoint_list[0].ip}@@"
                    },
                    "timeout_secs": "",
                    "type": "CHECK_LOGIN",
                    "variable_list": [],
                    "uuid": "d1c95f38-1877-47f6-8233-80d11ee6f6c5"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                  "uuid": "29159663-0a9e-4f97-98e4-01d5f4978896"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "39d98cd4-5475-46b0-87cf-6fa414cba73d"
              },
              "message_list": [],
              "name": "action_poweron"
            },
            {
              "description": "System action for suspending an application",
              "type": "system",
              "uuid": "eefada09-59f4-4a00-83d0-db01b2483891",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__DAG_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "8f87cdc2-bfdd-4813-a1c5-68ff3a46f483"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                  "uuid": "8f87cdc2-bfdd-4813-a1c5-68ff3a46f483"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "26ff79c2-9995-4f26-bb02-cda5446ce871"
              },
              "message_list": [],
              "name": "action_suspend"
            },
            {
              "description": "System action for modifying an application",
              "type": "system",
              "uuid": "0cf04ccb-3ff4-4f9d-b65e-b8e895e0ad4e",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__DAG_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "64daa070-b4fd-41be-b2dd-2af5fc77c53a"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                  "uuid": "64daa070-b4fd-41be-b2dd-2af5fc77c53a"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "5efc58d3-2c01-42a1-9e2b-8b7366292ca4"
              },
              "message_list": [],
              "name": "action_modify"
            },
            {
              "description": "System action for powering off an application",
              "type": "system",
              "uuid": "b378300a-0a25-4f86-b27b-6700da767d46",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Nutanix_Operation_PowerOff_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "78a89789-a497-4d07-9210-fe586e9a480f"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "82ed25ea-d114-4c31-a135-6609e7337986"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Nutanix_Operation_PowerOff_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "GENERIC_OPERATION"
                    },
                    "timeout_secs": "",
                    "type": "VMOPERATION_NUTANIX",
                    "variable_list": [],
                    "uuid": "78a89789-a497-4d07-9210-fe586e9a480f"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                  "uuid": "82ed25ea-d114-4c31-a135-6609e7337986"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "d71665d8-30af-433f-a178-34a2182ff5a3"
              },
              "message_list": [],
              "name": "action_poweroff"
            },
            {
              "description": "System action for deleting an application. Deletes physical machines as well",
              "type": "system",
              "uuid": "e0ec0abe-09e2-4ddb-af87-b859d77fbeed",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Nutanix_Delete_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "20a27e2a-637b-4880-a4f3-f6ebb2fd0e8e"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "814329b7-4ee2-47c6-a620-eaca48df36d7"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Nutanix_Delete_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DELETE_NUTANIX",
                    "variable_list": [],
                    "uuid": "20a27e2a-637b-4880-a4f3-f6ebb2fd0e8e"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                  "uuid": "814329b7-4ee2-47c6-a620-eaca48df36d7"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "c45c07c0-a1fe-4e10-a294-be3c54abb14a"
              },
              "message_list": [],
              "name": "action_delete"
            },
            {
              "description": "System action for deleting an application. Does not delete physical machines",
              "type": "system",
              "uuid": "c2c20128-213a-401e-b409-431bc06217af",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Substrate_Element_Delete_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "75257949-0940-4a11-bb27-2efadb5666a4"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "fe1d3e6d-25a6-446e-8464-ffe29d27ee73"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Substrate_Element_Delete_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "SOFT_DELETE_ELEMENT",
                    "variable_list": [],
                    "uuid": "75257949-0940-4a11-bb27-2efadb5666a4"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                  "uuid": "fe1d3e6d-25a6-446e-8464-ffe29d27ee73"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "674a9acb-0ce8-484c-ac15-ec4e5db43ca8"
              },
              "message_list": [],
              "name": "action_soft_delete"
            }
          ],
          "type": "AHV_VM",
          "name": "AHVReplicaSet",
          "state": "ACTIVE",
          "readiness_probe": {
            "connection_type": "SSH",
            "disable_readiness_probe": false,
            "timeout_secs": "60",
            "address": "@@{platform.status.resources.nic_list[0].ip_endpoint_list[0].ip}@@",
            "connection_port": 22,
            "login_credential_local_reference": {
              "kind": "app_credential",
              "name": "CENTOS",
              "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
            }
          },
          "editables": {
            "readiness_probe": {
              "connection_port": true,
              "timeout_secs": true
            },
            "create_spec": {
              "name": true,
              "resources": {
                "nic_list": {
                  "0": {
                    "subnet_reference": true
                  }
                },
                "num_vcpus_per_socket": true,
                "num_sockets": true,
                "memory_size_mib": true,
                "guest_customization": true,
                "disk_list": {
                  "0": {
                    "device_properties": {
                      "disk_address": {}
                    }
                  }
                }
              }
            }
          },
          "os_type": "Linux",
          "message_list": [],
          "create_spec": {
            "name": "MongoReplicaSetVM",
            "availability_zone_reference": null,
            "backup_policy": null,
            "type": "",
            "cluster_reference": null,
            "resources": {
              "nic_list": [
                {
                  "nic_type": "NORMAL_NIC",
                  "ip_endpoint_list": [],
                  "network_function_chain_reference": null,
                  "network_function_nic_type": "INGRESS",
                  "mac_address": "",
                  "subnet_reference": {
                    "kind": "subnet",
                    "type": "",
                    "name": "",
                    "uuid": "a6bb0e0b-fcb0-424c-95b3-5c700d11bdb8"
                  },
                  "type": ""
                }
              ],
              "power_state": "ON",
              "guest_tools": null,
              "num_vcpus_per_socket": 1,
              "num_sockets": 2,
              "gpu_list": [],
              "memory_size_mib": 4096,
              "parent_reference": null,
              "hardware_clock_timezone": "",
              "guest_customization": null,
              "type": "",
              "boot_config": {
                "boot_device": {
                  "type": "",
                  "disk_address": {
                    "type": "",
                    "device_index": 0,
                    "adapter_type": "SCSI"
                  }
                },
                "type": "",
                "mac_address": ""
              },
              "disk_list": [
                {
                  "data_source_reference": {
                    "kind": "image",
                    "type": "",
                    "name": "CentOS Server v7",
                    "uuid": "cb241054-89c4-40c4-8872-dcdbf7fa104d"
                  },
                  "type": "",
                  "disk_size_mib": 0,
                  "device_properties": {
                    "type": "",
                    "disk_address": {
                      "type": "",
                      "device_index": 0,
                      "adapter_type": "SCSI"
                    },
                    "device_type": "DISK"
                  }
                }
              ]
            }
          },
          "variable_list": [],
          "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
        }
      ],
      "credential_definition_list": [
        {
          "username": "root",
          "description": "",
          "secret": {
            "attrs": {
              "is_secret_modified": false,
              "secret_reference": {}
            }
          },
          "name": "CENTOS",
          "state": "ACTIVE",
          "editables": {
            "username": true,
            "secret": true
          },
          "type": "PASSWORD",
          "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
        }
      ],
      "app_profile_list": [
        {
          "deployment_create_list": [
            {
              "description": "",
              "action_list": [
                {
                  "description": "System action for creating an application",
                  "type": "system",
                  "uuid": "e0c9d841-13cf-4007-95ad-61d1aa841ac3",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "9b1e9738_deployment",
                          "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Provision_d7089f69_f4e4_4bae_871f_db099bd166a9",
                            "uuid": "3115be36-ec8b-4bd2-aed7-12fb528a932d"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "0020284a-82e0-40c5-a41b-7704319fd591"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__install_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "4d11a327-cf64-4289-8036-34cb7054c17e"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "d4786367-86b5-4172-b301-19916e7706ab"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "8f53c013-6581-44f2-9740-e2062c049674"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__create_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                                "uuid": "d4786367-86b5-4172-b301-19916e7706ab"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "d23d8f9d-f30f-4d36-b6e7-7c4814c4ceed",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                                "uuid": "8f53c013-6581-44f2-9740-e2062c049674"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__install_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                                "uuid": "4d11a327-cf64-4289-8036-34cb7054c17e"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "b3b318b6-47c9-4c93-ac72-8da246937101",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__create_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                                "uuid": "d4786367-86b5-4172-b301-19916e7706ab"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__DE_Provision_d7089f69_f4e4_4bae_871f_db099bd166a9",
                                "uuid": "3115be36-ec8b-4bd2-aed7-12fb528a932d"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "62da96b3-c271-4ae2-a4c7-2666b3462796",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__create_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                                "uuid": "0020284a-82e0-40c5-a41b-7704319fd591"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__create_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                                "uuid": "0020284a-82e0-40c5-a41b-7704319fd591"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "4d7877ec-de44-4bd5-ad54-417595e3d69c",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__install_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                                "uuid": "4d11a327-cf64-4289-8036-34cb7054c17e"
                              }
                            }
                          ],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "6c080e02-d647-48b6-a323-ed0c648041e1"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "9b1e9738_deployment",
                          "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__DE_Provision_d7089f69_f4e4_4bae_871f_db099bd166a9",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": ""
                        },
                        "timeout_secs": "",
                        "type": "CREATE_DEPLOYMENT_ELEMENT",
                        "variable_list": [],
                        "uuid": "3115be36-ec8b-4bd2-aed7-12fb528a932d"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_substrate",
                          "name": "AHVRouter",
                          "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__create_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "deaf522d-a578-48f2-b281-e446ee769188"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "0020284a-82e0-40c5-a41b-7704319fd591"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_package",
                          "name": "AHVRouterPackage",
                          "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__install_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "9b1e9738_deployment",
                            "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "f7c3e8ac-cfcc-4a79-b55f-c824b6c6dac7"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "4d11a327-cf64-4289-8036-34cb7054c17e"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_Router",
                          "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__create_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "9b1e9738_deployment",
                            "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "b9357b85_runbook",
                            "uuid": "c05fe841-6938-42a8-88c9-7f33e6d7d9f4"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "d4786367-86b5-4172-b301-19916e7706ab"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_Router",
                          "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "9b1e9738_deployment",
                            "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "d9638df2_runbook",
                            "uuid": "5c819c37-f08f-442e-af24-e72806300a94"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "8f53c013-6581-44f2-9740-e2062c049674"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                      "uuid": "6c080e02-d647-48b6-a323-ed0c648041e1"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "13b9fef0-aed8-4d37-941c-200d12253418"
                  },
                  "message_list": [],
                  "name": "action_create"
                },
                {
                  "description": "System action for starting an application",
                  "type": "system",
                  "uuid": "88a59913-3015-4c6b-8185-4359f7068831",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "9b1e9738_deployment",
                          "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "118032ff-992f-41bf-a5e8-22c6fb879350"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "eb11c1b0-2425-4f80-8d82-25aa009a2ea9"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__start_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                                "uuid": "118032ff-992f-41bf-a5e8-22c6fb879350"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "0b29e29e-d9de-42e9-aede-8204e1183f74",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                                "uuid": "eb11c1b0-2425-4f80-8d82-25aa009a2ea9"
                              }
                            }
                          ],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "abe0051e-e427-4ca3-acce-d77da0ecc089"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_substrate",
                          "name": "AHVRouter",
                          "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__start_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "5cdcc37b-17ca-49d7-a065-d0e8824c9176"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "118032ff-992f-41bf-a5e8-22c6fb879350"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_Router",
                          "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "9b1e9738_deployment",
                            "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "d9638df2_runbook",
                            "uuid": "5c819c37-f08f-442e-af24-e72806300a94"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "eb11c1b0-2425-4f80-8d82-25aa009a2ea9"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                      "uuid": "abe0051e-e427-4ca3-acce-d77da0ecc089"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "f4f1eb40-51b6-418a-b5a5-372b706c9052"
                  },
                  "message_list": [],
                  "name": "action_start"
                },
                {
                  "description": "System action for stopping an application",
                  "type": "system",
                  "uuid": "a39b4f69-626d-4c81-969c-288fa031a20b",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "9b1e9738_deployment",
                          "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "00dc3830-0278-4018-851d-4340950235e7"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "cd4ae0ca-3764-4be1-8de9-4dc494d2719a"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                                "uuid": "00dc3830-0278-4018-851d-4340950235e7"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "39b628a3-3937-4f77-a208-25db48b39055",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__stop_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                                "uuid": "cd4ae0ca-3764-4be1-8de9-4dc494d2719a"
                              }
                            }
                          ],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "205dcb65-219a-4b44-8ba7-2bb7550acd78"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_Router",
                          "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "9b1e9738_deployment",
                            "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "507a01b5_runbook",
                            "uuid": "3496374f-c76e-4ba7-bc72-53e1a0475fe3"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "00dc3830-0278-4018-851d-4340950235e7"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_substrate",
                          "name": "AHVRouter",
                          "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__stop_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "b1596526-1c86-439a-bc55-2c21dc7bd5f4"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "cd4ae0ca-3764-4be1-8de9-4dc494d2719a"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                      "uuid": "205dcb65-219a-4b44-8ba7-2bb7550acd78"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "fb6ffd40-2db3-4e13-a679-d86669e4379d"
                  },
                  "message_list": [],
                  "name": "action_stop"
                },
                {
                  "description": "System action for deleting an application. Deletes physical machines as well",
                  "type": "system",
                  "uuid": "1c29c3ff-71a3-4164-9db4-020cb8613207",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "9b1e9738_deployment",
                          "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "78dc3667-17c1-4958-8499-d59ded55d19b"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "02e633fd-aa6a-426a-9b57-c35847c9ec78"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__uninstall_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "d6de87a9-113a-4bb3-9ada-0a412c815c75"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "599265de-1065-4bc6-8c3f-e17e7b0428b7"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Delete_d7089f69_f4e4_4bae_871f_db099bd166a9",
                            "uuid": "449cd0d5-bb08-4839-8325-da6379f68a72"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                                "uuid": "78dc3667-17c1-4958-8499-d59ded55d19b"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "34f46328-b757-420e-9f84-7cdb2667afac",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                                "uuid": "02e633fd-aa6a-426a-9b57-c35847c9ec78"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                                "uuid": "02e633fd-aa6a-426a-9b57-c35847c9ec78"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "2d05b59f-5170-482a-9e46-9ecdaab94042",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__uninstall_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                                "uuid": "d6de87a9-113a-4bb3-9ada-0a412c815c75"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__uninstall_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                                "uuid": "d6de87a9-113a-4bb3-9ada-0a412c815c75"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "ffca0033-14c0-4142-8410-88514a63fb90",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                                "uuid": "599265de-1065-4bc6-8c3f-e17e7b0428b7"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                                "uuid": "599265de-1065-4bc6-8c3f-e17e7b0428b7"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "ca3c081b-8dd6-4923-8492-dd7218af88ee",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__DE_Delete_d7089f69_f4e4_4bae_871f_db099bd166a9",
                                "uuid": "449cd0d5-bb08-4839-8325-da6379f68a72"
                              }
                            }
                          ],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "8521fecb-d87b-4f56-8697-04df554c4fc3"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_Router",
                          "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "9b1e9738_deployment",
                            "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "507a01b5_runbook",
                            "uuid": "3496374f-c76e-4ba7-bc72-53e1a0475fe3"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "78dc3667-17c1-4958-8499-d59ded55d19b"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_Router",
                          "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "9b1e9738_deployment",
                            "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "10af4ae1_runbook",
                            "uuid": "c6f4df33-07f7-4a79-b87b-7fe3ec243b48"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "02e633fd-aa6a-426a-9b57-c35847c9ec78"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_package",
                          "name": "AHVRouterPackage",
                          "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__uninstall_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "9b1e9738_deployment",
                            "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "48b7c968-32a8-48d9-8bfa-9ba1f9a4da1e"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "d6de87a9-113a-4bb3-9ada-0a412c815c75"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_substrate",
                          "name": "AHVRouter",
                          "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "39f030aa-de7a-4bd0-b4bc-8ca45308b4cf"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "599265de-1065-4bc6-8c3f-e17e7b0428b7"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "9b1e9738_deployment",
                          "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__DE_Delete_d7089f69_f4e4_4bae_871f_db099bd166a9",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": ""
                        },
                        "timeout_secs": "",
                        "type": "DELETE_DEPLOYMENT_ELEMENT",
                        "variable_list": [],
                        "uuid": "449cd0d5-bb08-4839-8325-da6379f68a72"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                      "uuid": "8521fecb-d87b-4f56-8697-04df554c4fc3"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "99728e12-911b-4641-88e5-959c81682c5e"
                  },
                  "message_list": [],
                  "name": "action_delete"
                },
                {
                  "description": "System action for scaleout",
                  "type": "system",
                  "uuid": "b102641f-9aaf-4f00-ba11-3f4ea81d5d27",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "9b1e9738_deployment",
                          "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__Deployment_Scaleout_d7089f69_f4e4_4bae_871f_db099bd166a9",
                            "uuid": "13041629-16dc-4b6e-bf5e-f5788ebaf394"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "633fe0ed-1301-4b72-861c-535c07e01329"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "9b1e9738_deployment",
                          "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__Deployment_Scaleout_d7089f69_f4e4_4bae_871f_db099bd166a9",
                        "state": "ACTIVE",
                        "attrs": {
                          "scaling_count": "@@{scaling_count}@@",
                          "type": "",
                          "scaling_type": "SCALEOUT"
                        },
                        "timeout_secs": "",
                        "type": "SCALING",
                        "variable_list": [],
                        "uuid": "13041629-16dc-4b6e-bf5e-f5788ebaf394"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                      "uuid": "633fe0ed-1301-4b72-861c-535c07e01329"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "ea9df1aa-2a73-44d9-a1fd-f96a7d7be906"
                  },
                  "message_list": [],
                  "name": "action_scaleout"
                },
                {
                  "description": "System action for scalein",
                  "type": "system",
                  "uuid": "8c8a6114-0bfe-4abf-97d8-c4debae7902f",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "9b1e9738_deployment",
                          "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__Deployment_Scalein_d7089f69_f4e4_4bae_871f_db099bd166a9",
                            "uuid": "2d0cbcb5-3f66-4e34-b419-935c9051c14d"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "eda7bce2-064b-44f7-9fad-4a8c88ff59a8"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "9b1e9738_deployment",
                          "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__Deployment_Scalein_d7089f69_f4e4_4bae_871f_db099bd166a9",
                        "state": "ACTIVE",
                        "attrs": {
                          "scaling_count": "@@{scaling_count}@@",
                          "type": "",
                          "scaling_type": "SCALEIN"
                        },
                        "timeout_secs": "",
                        "type": "SCALING",
                        "variable_list": [],
                        "uuid": "2d0cbcb5-3f66-4e34-b419-935c9051c14d"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                      "uuid": "eda7bce2-064b-44f7-9fad-4a8c88ff59a8"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "81b290db-0198-47ac-a07f-a3f6bf857103"
                  },
                  "message_list": [],
                  "name": "action_scalein"
                },
                {
                  "description": "System action for deleting an application. Does not delete physical machines",
                  "type": "system",
                  "uuid": "84d9381c-2bbc-47ba-bd4a-e0a0a6ceac0d",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "9b1e9738_deployment",
                          "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "b57f87ad-5279-40c0-8a32-ea3f54ed9b82"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__Package_Element_Delete_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "517430de-756b-4e6f-9157-0cd7344950e9"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "39cfcf87-68e7-4d9e-907a-a95782d29b00"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Soft_Delete_d7089f69_f4e4_4bae_871f_db099bd166a9",
                            "uuid": "4c1ce43f-bf18-4b83-ac12-a1d5d2d53bef"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__soft_delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                                "uuid": "b57f87ad-5279-40c0-8a32-ea3f54ed9b82"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "0e30aca9-574a-40e9-b256-242a0d1eadd0",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__Package_Element_Delete_1299fd60_5173_4b37_8f46_3374f51e45dc",
                                "uuid": "517430de-756b-4e6f-9157-0cd7344950e9"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__Package_Element_Delete_1299fd60_5173_4b37_8f46_3374f51e45dc",
                                "uuid": "517430de-756b-4e6f-9157-0cd7344950e9"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "f609d74c-aac1-48af-a380-7e3b8607d594",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__soft_delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                                "uuid": "39cfcf87-68e7-4d9e-907a-a95782d29b00"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__soft_delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                                "uuid": "39cfcf87-68e7-4d9e-907a-a95782d29b00"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "07f96864-f4ca-42c8-8e6d-669462293f7f",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__DE_Soft_Delete_d7089f69_f4e4_4bae_871f_db099bd166a9",
                                "uuid": "4c1ce43f-bf18-4b83-ac12-a1d5d2d53bef"
                              }
                            }
                          ],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "9aeff825-f540-4838-878d-bb8f9a8d2efa"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_Router",
                          "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__soft_delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "9b1e9738_deployment",
                            "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "67c7551e-6768-46db-bde8-e28af739c02f"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "b57f87ad-5279-40c0-8a32-ea3f54ed9b82"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_package",
                          "name": "AHVRouterPackage",
                          "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__Package_Element_Delete_1299fd60_5173_4b37_8f46_3374f51e45dc",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": ""
                        },
                        "timeout_secs": "",
                        "type": "SOFT_DELETE_ELEMENT",
                        "variable_list": [],
                        "uuid": "517430de-756b-4e6f-9157-0cd7344950e9"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_substrate",
                          "name": "AHVRouter",
                          "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__soft_delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "50a06689-a733-4df5-910b-847260cca798"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "39cfcf87-68e7-4d9e-907a-a95782d29b00"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "9b1e9738_deployment",
                          "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__DE_Soft_Delete_d7089f69_f4e4_4bae_871f_db099bd166a9",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": ""
                        },
                        "timeout_secs": "",
                        "type": "SOFT_DELETE_DEPLOYMENT_ELEMENT",
                        "variable_list": [],
                        "uuid": "4c1ce43f-bf18-4b83-ac12-a1d5d2d53bef"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_d7089f69_f4e4_4bae_871f_db099bd166a9",
                      "uuid": "9aeff825-f540-4838-878d-bb8f9a8d2efa"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "515cd54b-4db7-4343-9dfa-55f0d9d7fa9d"
                  },
                  "message_list": [],
                  "name": "action_soft_delete"
                }
              ],
              "message_list": [],
              "editables": {},
              "name": "9b1e9738_deployment",
              "state": "ACTIVE",
              "max_replicas": "1",
              "package_local_reference_list": [
                {
                  "kind": "app_package",
                  "name": "AHVRouterPackage",
                  "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                }
              ],
              "substrate_local_reference": {
                "kind": "app_substrate",
                "name": "AHVRouter",
                "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
              },
              "min_replicas": "1",
              "variable_list": [],
              "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
            },
            {
              "description": "",
              "action_list": [
                {
                  "description": "System action for creating an application",
                  "type": "system",
                  "uuid": "027331ac-d961-48f7-bb38-b9e72be87a07",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "e0823432_deployment",
                          "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Provision_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                            "uuid": "4884593e-11ce-484c-a162-3a03eba68da1"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "1d8ba222-c873-4153-96b0-8a0796f9495a"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__install_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                            "uuid": "18690380-cffa-4005-b927-b754b3d6a127"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "641a3f06-d665-4c6f-832d-5a2be65b8104"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "9de77ea3-20e6-4141-ab54-8727375f2356"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__create_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                                "uuid": "641a3f06-d665-4c6f-832d-5a2be65b8104"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "34661424-137c-4f55-8ca1-8c9d7cbc7529",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                                "uuid": "9de77ea3-20e6-4141-ab54-8727375f2356"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__install_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                                "uuid": "18690380-cffa-4005-b927-b754b3d6a127"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "57aa7eb2-23ab-479f-91b0-96dfe284855d",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__create_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                                "uuid": "641a3f06-d665-4c6f-832d-5a2be65b8104"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__DE_Provision_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                                "uuid": "4884593e-11ce-484c-a162-3a03eba68da1"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "42bcab7e-72d0-4264-8128-746efe49608d",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__create_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                                "uuid": "1d8ba222-c873-4153-96b0-8a0796f9495a"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__create_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                                "uuid": "1d8ba222-c873-4153-96b0-8a0796f9495a"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "a1a19811-292c-47b4-ad89-d25f8f457a74",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__install_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                                "uuid": "18690380-cffa-4005-b927-b754b3d6a127"
                              }
                            }
                          ],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "c0c3fe0f-1e6a-44ae-8808-dc09e2d001de"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "e0823432_deployment",
                          "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__DE_Provision_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": ""
                        },
                        "timeout_secs": "",
                        "type": "CREATE_DEPLOYMENT_ELEMENT",
                        "variable_list": [],
                        "uuid": "4884593e-11ce-484c-a162-3a03eba68da1"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_substrate",
                          "name": "AHVConfigSet",
                          "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__create_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "16c52300-6568-4f9d-8abf-7fad590a18ac"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "1d8ba222-c873-4153-96b0-8a0796f9495a"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_package",
                          "name": "AHVConfigSetPackage",
                          "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__install_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "e0823432_deployment",
                            "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                            "uuid": "74248961-1408-4b07-8401-9f96b80bb049"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "18690380-cffa-4005-b927-b754b3d6a127"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_ConfigSet",
                          "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__create_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "e0823432_deployment",
                            "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "e8608054_runbook",
                            "uuid": "5dd0f77d-1c9e-4bbb-9d61-50c834876150"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "641a3f06-d665-4c6f-832d-5a2be65b8104"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_ConfigSet",
                          "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "e0823432_deployment",
                            "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "d04295dc_runbook",
                            "uuid": "b4ac0257-a074-4e37-9f8b-9df8a30127f1"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "9de77ea3-20e6-4141-ab54-8727375f2356"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                      "uuid": "c0c3fe0f-1e6a-44ae-8808-dc09e2d001de"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "3cdaae51-51ae-4d31-ad86-03e92348d78d"
                  },
                  "message_list": [],
                  "name": "action_create"
                },
                {
                  "description": "System action for starting an application",
                  "type": "system",
                  "uuid": "278207c6-da06-4943-a4c9-41c8d8eb356c",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "e0823432_deployment",
                          "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "343e36a2-f4b5-4c3d-a01b-604cf0f67c73"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "bc0ab653-8db0-4f81-9359-a9ae3b6734d8"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__start_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                                "uuid": "343e36a2-f4b5-4c3d-a01b-604cf0f67c73"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "5a6a1d0c-e22d-49b2-8808-f4738a49c236",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                                "uuid": "bc0ab653-8db0-4f81-9359-a9ae3b6734d8"
                              }
                            }
                          ],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "2da33bbc-6bbe-42cd-8578-3383312eb013"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_substrate",
                          "name": "AHVConfigSet",
                          "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__start_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "3cf91285-df41-4ae8-9a82-d1d1fae9de0e"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "343e36a2-f4b5-4c3d-a01b-604cf0f67c73"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_ConfigSet",
                          "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "e0823432_deployment",
                            "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "d04295dc_runbook",
                            "uuid": "b4ac0257-a074-4e37-9f8b-9df8a30127f1"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "bc0ab653-8db0-4f81-9359-a9ae3b6734d8"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                      "uuid": "2da33bbc-6bbe-42cd-8578-3383312eb013"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "1a12026e-9690-4f84-b2d7-302fea327152"
                  },
                  "message_list": [],
                  "name": "action_start"
                },
                {
                  "description": "System action for stopping an application",
                  "type": "system",
                  "uuid": "45dc0670-42cd-4861-8133-69bb77cfe2ce",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "e0823432_deployment",
                          "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "ae2257f3-5c6c-4ffd-9fce-d75df2a5a071"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "3e25d233-2cf4-4174-b7fa-23d8bdf1f6cf"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                                "uuid": "ae2257f3-5c6c-4ffd-9fce-d75df2a5a071"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "b7cf2238-9f2f-4b68-941e-489b802a2646",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__stop_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                                "uuid": "3e25d233-2cf4-4174-b7fa-23d8bdf1f6cf"
                              }
                            }
                          ],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "233ecd8b-9fc8-4975-8c12-1d30e597ee48"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_ConfigSet",
                          "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "e0823432_deployment",
                            "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "c93943b8_runbook",
                            "uuid": "c2ba6f77-45ff-4d0c-80bc-6a13a88bac4e"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "ae2257f3-5c6c-4ffd-9fce-d75df2a5a071"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_substrate",
                          "name": "AHVConfigSet",
                          "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__stop_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "b4df5ceb-d327-4062-9d7b-39e0a1b5981a"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "3e25d233-2cf4-4174-b7fa-23d8bdf1f6cf"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                      "uuid": "233ecd8b-9fc8-4975-8c12-1d30e597ee48"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "12805dcf-36dd-4b57-8f46-6612e5c93c4a"
                  },
                  "message_list": [],
                  "name": "action_stop"
                },
                {
                  "description": "System action for deleting an application. Deletes physical machines as well",
                  "type": "system",
                  "uuid": "e1fb8f11-790a-422d-bac2-1848a37bf2cb",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "e0823432_deployment",
                          "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "ebb7dd02-6c61-4dae-94b0-ae6c747b24a3"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "105feea1-bb12-4d61-ba82-e1b8f7c7021a"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__uninstall_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                            "uuid": "464d1e36-9d80-44d5-b9f3-e0364aec8011"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "07aded76-abfe-403d-8ff1-7dd8a8af5f83"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Delete_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                            "uuid": "2714d81f-9cf4-49bd-a92b-db1534c1c500"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                                "uuid": "ebb7dd02-6c61-4dae-94b0-ae6c747b24a3"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "9f1a7eef-a0ac-47b4-a936-353f4e6c5d17",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                                "uuid": "105feea1-bb12-4d61-ba82-e1b8f7c7021a"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                                "uuid": "105feea1-bb12-4d61-ba82-e1b8f7c7021a"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "4c96663e-9074-4282-ac96-20763f4bd1b3",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__uninstall_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                                "uuid": "464d1e36-9d80-44d5-b9f3-e0364aec8011"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                                "uuid": "07aded76-abfe-403d-8ff1-7dd8a8af5f83"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "64ab69c5-5bea-4d9b-a887-6627ba357ea3",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__DE_Delete_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                                "uuid": "2714d81f-9cf4-49bd-a92b-db1534c1c500"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__uninstall_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                                "uuid": "464d1e36-9d80-44d5-b9f3-e0364aec8011"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "23ab5eda-5770-4e27-b2a9-854d2260ceec",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                                "uuid": "07aded76-abfe-403d-8ff1-7dd8a8af5f83"
                              }
                            }
                          ],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "2f7c96ab-aecd-4a6d-9096-1c5c9fd7ea19"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_ConfigSet",
                          "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "e0823432_deployment",
                            "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "c93943b8_runbook",
                            "uuid": "c2ba6f77-45ff-4d0c-80bc-6a13a88bac4e"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "ebb7dd02-6c61-4dae-94b0-ae6c747b24a3"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_ConfigSet",
                          "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "e0823432_deployment",
                            "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "fd485dc4_runbook",
                            "uuid": "bd261c01-2c76-46fd-a615-846ed9b187b1"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "105feea1-bb12-4d61-ba82-e1b8f7c7021a"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_package",
                          "name": "AHVConfigSetPackage",
                          "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__uninstall_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "e0823432_deployment",
                            "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                            "uuid": "30e18a00-af7c-4abe-b6e9-5d05a611972a"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "464d1e36-9d80-44d5-b9f3-e0364aec8011"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_substrate",
                          "name": "AHVConfigSet",
                          "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "4d2b8fca-dccf-4d06-93b9-eb6abbaa71de"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "07aded76-abfe-403d-8ff1-7dd8a8af5f83"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "e0823432_deployment",
                          "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__DE_Delete_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": ""
                        },
                        "timeout_secs": "",
                        "type": "DELETE_DEPLOYMENT_ELEMENT",
                        "variable_list": [],
                        "uuid": "2714d81f-9cf4-49bd-a92b-db1534c1c500"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                      "uuid": "2f7c96ab-aecd-4a6d-9096-1c5c9fd7ea19"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "12aeeb19-6e43-4243-8a76-840dc7ac9296"
                  },
                  "message_list": [],
                  "name": "action_delete"
                },
                {
                  "description": "System action for scaleout",
                  "type": "system",
                  "uuid": "1488ea1d-dbad-41aa-b84e-fe6f47dcbc93",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "e0823432_deployment",
                          "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__Deployment_Scaleout_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                            "uuid": "ec50d2d4-2148-4063-a1b4-64f9edc1bdd9"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "89f30cfe-3414-429f-8fa0-b9f566e30c8a"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "e0823432_deployment",
                          "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__Deployment_Scaleout_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                        "state": "ACTIVE",
                        "attrs": {
                          "scaling_count": "@@{scaling_count}@@",
                          "type": "",
                          "scaling_type": "SCALEOUT"
                        },
                        "timeout_secs": "",
                        "type": "SCALING",
                        "variable_list": [],
                        "uuid": "ec50d2d4-2148-4063-a1b4-64f9edc1bdd9"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                      "uuid": "89f30cfe-3414-429f-8fa0-b9f566e30c8a"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "fba8474d-8cba-4ad4-9142-f7591e1665e6"
                  },
                  "message_list": [],
                  "name": "action_scaleout"
                },
                {
                  "description": "System action for scalein",
                  "type": "system",
                  "uuid": "1c7e8279-b770-4756-b895-0ed069d3d61e",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "e0823432_deployment",
                          "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__Deployment_Scalein_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                            "uuid": "0d0386d2-c261-43f5-a9c5-44ca7db96b11"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "01fbafca-e9e9-455f-9b10-b06e20000886"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "e0823432_deployment",
                          "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__Deployment_Scalein_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                        "state": "ACTIVE",
                        "attrs": {
                          "scaling_count": "@@{scaling_count}@@",
                          "type": "",
                          "scaling_type": "SCALEIN"
                        },
                        "timeout_secs": "",
                        "type": "SCALING",
                        "variable_list": [],
                        "uuid": "0d0386d2-c261-43f5-a9c5-44ca7db96b11"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                      "uuid": "01fbafca-e9e9-455f-9b10-b06e20000886"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "bdaad210-e008-4899-ae52-f7ddcb32e05a"
                  },
                  "message_list": [],
                  "name": "action_scalein"
                },
                {
                  "description": "System action for deleting an application. Does not delete physical machines",
                  "type": "system",
                  "uuid": "55f584cd-5da9-4902-900a-90221d14ecc8",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "e0823432_deployment",
                          "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "a89e1c84-1c59-4570-b3e8-f69dfa269555"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__Package_Element_Delete_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                            "uuid": "d9db0f9b-929f-4011-956b-9456243d1009"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "653082c7-0cad-4d5c-bfda-c4dad2114540"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Soft_Delete_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                            "uuid": "bcd41931-1ca7-4c3b-be0f-ef5e75083a40"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__soft_delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                                "uuid": "a89e1c84-1c59-4570-b3e8-f69dfa269555"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "96c2cd2b-a0bf-47e7-91a0-7fa9390b1194",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__Package_Element_Delete_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                                "uuid": "d9db0f9b-929f-4011-956b-9456243d1009"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__soft_delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                                "uuid": "653082c7-0cad-4d5c-bfda-c4dad2114540"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "62366fbe-da74-4424-abc8-1179cfe57c50",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__DE_Soft_Delete_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                                "uuid": "bcd41931-1ca7-4c3b-be0f-ef5e75083a40"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__Package_Element_Delete_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                                "uuid": "d9db0f9b-929f-4011-956b-9456243d1009"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "c55939a0-216f-4fc2-8a4b-e0e1a4416153",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__soft_delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                                "uuid": "653082c7-0cad-4d5c-bfda-c4dad2114540"
                              }
                            }
                          ],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "f8c6b876-b546-400a-96d2-e222f4e9251a"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_ConfigSet",
                          "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__soft_delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "e0823432_deployment",
                            "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "ae786a91-e961-47bb-afe7-05a0b7244131"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "a89e1c84-1c59-4570-b3e8-f69dfa269555"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_package",
                          "name": "AHVConfigSetPackage",
                          "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__Package_Element_Delete_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": ""
                        },
                        "timeout_secs": "",
                        "type": "SOFT_DELETE_ELEMENT",
                        "variable_list": [],
                        "uuid": "d9db0f9b-929f-4011-956b-9456243d1009"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_substrate",
                          "name": "AHVConfigSet",
                          "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__soft_delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "4bfd4ceb-6310-4fc9-b694-3568aff778bc"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "653082c7-0cad-4d5c-bfda-c4dad2114540"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "e0823432_deployment",
                          "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__DE_Soft_Delete_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": ""
                        },
                        "timeout_secs": "",
                        "type": "SOFT_DELETE_DEPLOYMENT_ELEMENT",
                        "variable_list": [],
                        "uuid": "bcd41931-1ca7-4c3b-be0f-ef5e75083a40"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                      "uuid": "f8c6b876-b546-400a-96d2-e222f4e9251a"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "a2bbe45e-3572-4f28-834b-5d857d092145"
                  },
                  "message_list": [],
                  "name": "action_soft_delete"
                }
              ],
              "message_list": [],
              "editables": {
                "min_replicas": true
              },
              "name": "e0823432_deployment",
              "state": "ACTIVE",
              "max_replicas": "1",
              "package_local_reference_list": [
                {
                  "kind": "app_package",
                  "name": "AHVConfigSetPackage",
                  "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                }
              ],
              "substrate_local_reference": {
                "kind": "app_substrate",
                "name": "AHVConfigSet",
                "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
              },
              "min_replicas": "1",
              "variable_list": [],
              "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
            },
            {
              "description": "",
              "action_list": [
                {
                  "description": "System action for creating an application",
                  "type": "system",
                  "uuid": "5f735772-b6c7-4d4c-843a-1de83732b9df",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "871434ae_deployment",
                          "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Provision_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                            "uuid": "8576999b-55dc-4492-8488-71c3cddf2a31"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "4e3eb5a7-e0e0-46ec-b6e4-1e9cdb429923"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__install_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                            "uuid": "af7b8174-dd80-422b-ad6d-51eade73f9f3"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "96a0c703-25bf-442f-98ac-9302d895ed7f"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "5ffe5f9c-92e4-4a9b-9a43-6c1427d4a9ab"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__create_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                                "uuid": "96a0c703-25bf-442f-98ac-9302d895ed7f"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "50b6cec8-fbd6-46d5-9185-009ed9cc7941",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                                "uuid": "5ffe5f9c-92e4-4a9b-9a43-6c1427d4a9ab"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__install_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                                "uuid": "af7b8174-dd80-422b-ad6d-51eade73f9f3"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "5d05dbab-84d2-446a-9f4d-51b0350b7e64",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__create_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                                "uuid": "96a0c703-25bf-442f-98ac-9302d895ed7f"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__DE_Provision_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                                "uuid": "8576999b-55dc-4492-8488-71c3cddf2a31"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "849ea041-fc4b-4683-8c9e-b72366caabf6",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__create_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                                "uuid": "4e3eb5a7-e0e0-46ec-b6e4-1e9cdb429923"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__create_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                                "uuid": "4e3eb5a7-e0e0-46ec-b6e4-1e9cdb429923"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "1b77c395-e57a-4143-a0ee-66202eaa5145",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__install_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                                "uuid": "af7b8174-dd80-422b-ad6d-51eade73f9f3"
                              }
                            }
                          ],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "35206904-cfa2-406d-84c1-7f4d19b59121"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "871434ae_deployment",
                          "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__DE_Provision_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": ""
                        },
                        "timeout_secs": "",
                        "type": "CREATE_DEPLOYMENT_ELEMENT",
                        "variable_list": [],
                        "uuid": "8576999b-55dc-4492-8488-71c3cddf2a31"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_substrate",
                          "name": "AHVReplicaSet",
                          "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__create_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "52b73e8a-27bd-41b3-8a7c-d5822b971228"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "4e3eb5a7-e0e0-46ec-b6e4-1e9cdb429923"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_package",
                          "name": "AHVReplicaSetPackage",
                          "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__install_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "871434ae_deployment",
                            "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                            "uuid": "015de7d5-5fe6-4d2a-82cc-2e08ad07cb68"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "af7b8174-dd80-422b-ad6d-51eade73f9f3"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_ReplicaSet",
                          "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__create_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "871434ae_deployment",
                            "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "e984fac1_runbook",
                            "uuid": "c4094263-6a43-46f6-9b5e-335b65860978"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "96a0c703-25bf-442f-98ac-9302d895ed7f"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_ReplicaSet",
                          "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "871434ae_deployment",
                            "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "6286b364_runbook",
                            "uuid": "b414ddcc-2221-4078-bac1-8c739a480a49"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "5ffe5f9c-92e4-4a9b-9a43-6c1427d4a9ab"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                      "uuid": "35206904-cfa2-406d-84c1-7f4d19b59121"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "614d4651-49d5-4847-a8b6-aa0129711476"
                  },
                  "message_list": [],
                  "name": "action_create"
                },
                {
                  "description": "System action for starting an application",
                  "type": "system",
                  "uuid": "8c1555cc-f58c-4498-8069-ad1ac989a991",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "871434ae_deployment",
                          "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "7541e6e6-b14f-4fa0-b6f8-3482a5af40e9"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "8169b8d7-f6b4-4699-91d3-8a264508284f"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__start_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                                "uuid": "7541e6e6-b14f-4fa0-b6f8-3482a5af40e9"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "822f032f-7a0a-46f5-9938-e69b9f22bc4a",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                                "uuid": "8169b8d7-f6b4-4699-91d3-8a264508284f"
                              }
                            }
                          ],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "03c1e38b-17d7-4d82-8f48-54d9b7f5f219"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_substrate",
                          "name": "AHVReplicaSet",
                          "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__start_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "39d98cd4-5475-46b0-87cf-6fa414cba73d"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "7541e6e6-b14f-4fa0-b6f8-3482a5af40e9"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_ReplicaSet",
                          "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "871434ae_deployment",
                            "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "6286b364_runbook",
                            "uuid": "b414ddcc-2221-4078-bac1-8c739a480a49"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "8169b8d7-f6b4-4699-91d3-8a264508284f"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                      "uuid": "03c1e38b-17d7-4d82-8f48-54d9b7f5f219"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "add0756f-6580-4566-bc4f-a11ccb74979b"
                  },
                  "message_list": [],
                  "name": "action_start"
                },
                {
                  "description": "System action for stopping an application",
                  "type": "system",
                  "uuid": "03b2601f-91e2-4fa0-8022-7110bd78e37d",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "871434ae_deployment",
                          "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "9d483542-4324-423b-99d3-b3d1a93bfa52"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "7c2b41ca-596c-4bc0-9b97-476ac1514e74"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                                "uuid": "9d483542-4324-423b-99d3-b3d1a93bfa52"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "19e8487b-7529-4f75-99b7-0c13001d7d92",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__stop_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                                "uuid": "7c2b41ca-596c-4bc0-9b97-476ac1514e74"
                              }
                            }
                          ],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "33e26a2b-4eb9-4f67-9b0a-949ef1bf4175"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_ReplicaSet",
                          "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "871434ae_deployment",
                            "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "ee3949e3_runbook",
                            "uuid": "706d1aab-530f-452d-8b33-15bfbdb2fdd7"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "9d483542-4324-423b-99d3-b3d1a93bfa52"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_substrate",
                          "name": "AHVReplicaSet",
                          "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__stop_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "d71665d8-30af-433f-a178-34a2182ff5a3"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "7c2b41ca-596c-4bc0-9b97-476ac1514e74"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                      "uuid": "33e26a2b-4eb9-4f67-9b0a-949ef1bf4175"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "28d18ccb-3a2b-412e-b814-b1a9306deed2"
                  },
                  "message_list": [],
                  "name": "action_stop"
                },
                {
                  "description": "System action for deleting an application. Deletes physical machines as well",
                  "type": "system",
                  "uuid": "4452c030-f548-4713-98d2-5489c9d0bc9e",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "871434ae_deployment",
                          "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "7c41272e-f91e-487e-bcc8-967c8fca9a1c"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "e8e468b3-f47b-4113-b89c-19700bc35103"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__uninstall_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                            "uuid": "3591e925-866e-41fa-a6df-02d3f189c486"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "cc2552ca-278e-4907-bb0a-dceacbe51ffc"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Delete_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                            "uuid": "580d1f68-969d-474c-af23-448fc46489fc"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                                "uuid": "7c41272e-f91e-487e-bcc8-967c8fca9a1c"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "a8ea20e2-db24-4510-8fd4-b5514cdfcbfd",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                                "uuid": "e8e468b3-f47b-4113-b89c-19700bc35103"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                                "uuid": "e8e468b3-f47b-4113-b89c-19700bc35103"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "9f7d3a97-b854-44ce-885a-35aaa2664f4a",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__uninstall_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                                "uuid": "3591e925-866e-41fa-a6df-02d3f189c486"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                                "uuid": "cc2552ca-278e-4907-bb0a-dceacbe51ffc"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "9e691244-bc59-46ad-8424-3a31135f9898",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__DE_Delete_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                                "uuid": "580d1f68-969d-474c-af23-448fc46489fc"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__uninstall_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                                "uuid": "3591e925-866e-41fa-a6df-02d3f189c486"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "fa8f67a4-3d02-41c4-ace0-0c4f3918ad87",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                                "uuid": "cc2552ca-278e-4907-bb0a-dceacbe51ffc"
                              }
                            }
                          ],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "2a03f33b-5198-4706-9561-516a2c9c8ace"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_ReplicaSet",
                          "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "871434ae_deployment",
                            "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "ee3949e3_runbook",
                            "uuid": "706d1aab-530f-452d-8b33-15bfbdb2fdd7"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "7c41272e-f91e-487e-bcc8-967c8fca9a1c"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_ReplicaSet",
                          "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "871434ae_deployment",
                            "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "96df4d94_runbook",
                            "uuid": "8d242223-afda-4c13-b35f-b9dff6717e5e"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "e8e468b3-f47b-4113-b89c-19700bc35103"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_package",
                          "name": "AHVReplicaSetPackage",
                          "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__uninstall_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "871434ae_deployment",
                            "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                            "uuid": "7a4f2ebd-ee07-4fec-973d-0a284ae72425"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "3591e925-866e-41fa-a6df-02d3f189c486"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_substrate",
                          "name": "AHVReplicaSet",
                          "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "c45c07c0-a1fe-4e10-a294-be3c54abb14a"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "cc2552ca-278e-4907-bb0a-dceacbe51ffc"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "871434ae_deployment",
                          "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__DE_Delete_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": ""
                        },
                        "timeout_secs": "",
                        "type": "DELETE_DEPLOYMENT_ELEMENT",
                        "variable_list": [],
                        "uuid": "580d1f68-969d-474c-af23-448fc46489fc"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                      "uuid": "2a03f33b-5198-4706-9561-516a2c9c8ace"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "07fbd97e-ce46-4a0e-bf71-01d97f3838ae"
                  },
                  "message_list": [],
                  "name": "action_delete"
                },
                {
                  "description": "System action for scaleout",
                  "type": "system",
                  "uuid": "5943eb78-e6c2-48fa-9de0-c11c6f335be5",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "871434ae_deployment",
                          "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__Deployment_Scaleout_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                            "uuid": "150f5b4f-8b59-4e4c-88f7-0102d86fce6a"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "4a0dd62e-ec85-4f60-8774-0c9dc713aa09"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "871434ae_deployment",
                          "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__Deployment_Scaleout_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                        "state": "ACTIVE",
                        "attrs": {
                          "scaling_count": "@@{scaling_count}@@",
                          "type": "",
                          "scaling_type": "SCALEOUT"
                        },
                        "timeout_secs": "",
                        "type": "SCALING",
                        "variable_list": [],
                        "uuid": "150f5b4f-8b59-4e4c-88f7-0102d86fce6a"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                      "uuid": "4a0dd62e-ec85-4f60-8774-0c9dc713aa09"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "89fbd974-3621-4bfc-84be-41df6afee44f"
                  },
                  "message_list": [],
                  "name": "action_scaleout"
                },
                {
                  "description": "System action for scalein",
                  "type": "system",
                  "uuid": "dbda8181-9179-4232-8f49-bd2fd9ac7eb2",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "871434ae_deployment",
                          "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__Deployment_Scalein_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                            "uuid": "f4e921e4-fc93-4fe3-9f7d-0e97881cc379"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "e4201360-6138-44ec-b38d-07011f70e610"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "871434ae_deployment",
                          "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__Deployment_Scalein_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                        "state": "ACTIVE",
                        "attrs": {
                          "scaling_count": "@@{scaling_count}@@",
                          "type": "",
                          "scaling_type": "SCALEIN"
                        },
                        "timeout_secs": "",
                        "type": "SCALING",
                        "variable_list": [],
                        "uuid": "f4e921e4-fc93-4fe3-9f7d-0e97881cc379"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                      "uuid": "e4201360-6138-44ec-b38d-07011f70e610"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "52d0aec0-f732-4d73-a614-89b3f8918321"
                  },
                  "message_list": [],
                  "name": "action_scalein"
                },
                {
                  "description": "System action for deleting an application. Does not delete physical machines",
                  "type": "system",
                  "uuid": "6e5d2975-7655-48b5-8367-01fbfd7aaa42",
                  "state": "ACTIVE",
                  "critical": true,
                  "attrs": {},
                  "runbook": {
                    "task_definition_list": [
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "871434ae_deployment",
                          "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "7cfd7f30-6a40-4318-b7a1-85696dc0d7a1"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__Package_Element_Delete_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                            "uuid": "76bb8a3c-b2c0-4ba9-8e06-dd7a5f12cb2a"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "5a831861-afbb-4580-98dc-d2b7f7901e79"
                          },
                          {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Soft_Delete_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                            "uuid": "167c29f2-f2ae-4af8-b098-0d6ea80c5281"
                          }
                        ],
                        "name": "SYS_GEN__Composite_DAG_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                        "state": "ACTIVE",
                        "attrs": {
                          "edges": [
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__soft_delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                                "uuid": "7cfd7f30-6a40-4318-b7a1-85696dc0d7a1"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "eb22a50d-fd4a-4433-8f99-117323dc5016",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__Package_Element_Delete_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                                "uuid": "76bb8a3c-b2c0-4ba9-8e06-dd7a5f12cb2a"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__soft_delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                                "uuid": "5a831861-afbb-4580-98dc-d2b7f7901e79"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "cdab8d19-707e-449e-ae86-7c8ef52150c1",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__DE_Soft_Delete_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                                "uuid": "167c29f2-f2ae-4af8-b098-0d6ea80c5281"
                              }
                            },
                            {
                              "from_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__Package_Element_Delete_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                                "uuid": "76bb8a3c-b2c0-4ba9-8e06-dd7a5f12cb2a"
                              },
                              "edge_type": "inherent",
                              "type": "",
                              "uuid": "dd91a767-a1a8-416b-b5dc-2d6732361ff5",
                              "to_task_reference": {
                                "kind": "app_task",
                                "name": "SYS_GEN__soft_delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                                "uuid": "5a831861-afbb-4580-98dc-d2b7f7901e79"
                              }
                            }
                          ],
                          "type": "DAG"
                        },
                        "timeout_secs": "",
                        "type": "DAG",
                        "variable_list": [],
                        "uuid": "e78d25b2-6a24-44b0-8087-909852c1d242"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_service",
                          "name": "Mongo_ReplicaSet",
                          "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__soft_delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "deployment_reference": {
                            "kind": "app_blueprint_deployment",
                            "name": "871434ae_deployment",
                            "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                          },
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "c0f5e1e4-bdb8-4981-baa5-7f01c4a753ad"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "7cfd7f30-6a40-4318-b7a1-85696dc0d7a1"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_package",
                          "name": "AHVReplicaSetPackage",
                          "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__Package_Element_Delete_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": ""
                        },
                        "timeout_secs": "",
                        "type": "SOFT_DELETE_ELEMENT",
                        "variable_list": [],
                        "uuid": "76bb8a3c-b2c0-4ba9-8e06-dd7a5f12cb2a"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_substrate",
                          "name": "AHVReplicaSet",
                          "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__soft_delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": "CALL_RUNBOOK",
                          "inarg_list": [],
                          "runbook_reference": {
                            "kind": "app_runbook",
                            "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "674a9acb-0ce8-484c-ac15-ec4e5db43ca8"
                          }
                        },
                        "timeout_secs": "",
                        "type": "CALL_RUNBOOK",
                        "variable_list": [],
                        "uuid": "5a831861-afbb-4580-98dc-d2b7f7901e79"
                      },
                      {
                        "target_any_local_reference": {
                          "kind": "app_blueprint_deployment",
                          "name": "871434ae_deployment",
                          "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                        },
                        "description": "",
                        "message_list": [],
                        "child_tasks_local_reference_list": [],
                        "name": "SYS_GEN__DE_Soft_Delete_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                        "state": "ACTIVE",
                        "attrs": {
                          "type": ""
                        },
                        "timeout_secs": "",
                        "type": "SOFT_DELETE_DEPLOYMENT_ELEMENT",
                        "variable_list": [],
                        "uuid": "167c29f2-f2ae-4af8-b098-0d6ea80c5281"
                      }
                    ],
                    "description": "",
                    "name": "SYS_GEN__Runbook_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                    "state": "ACTIVE",
                    "main_task_local_reference": {
                      "kind": "app_task",
                      "name": "SYS_GEN__Composite_DAG_Deployment_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                      "uuid": "e78d25b2-6a24-44b0-8087-909852c1d242"
                    },
                    "message_list": [],
                    "variable_list": [],
                    "uuid": "58df263a-4d1f-4254-9944-0cdd40ab5d43"
                  },
                  "message_list": [],
                  "name": "action_soft_delete"
                }
              ],
              "message_list": [],
              "editables": {
                "min_replicas": true
              },
              "name": "871434ae_deployment",
              "state": "ACTIVE",
              "max_replicas": "2",
              "package_local_reference_list": [
                {
                  "kind": "app_package",
                  "name": "AHVReplicaSetPackage",
                  "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                }
              ],
              "substrate_local_reference": {
                "kind": "app_substrate",
                "name": "AHVReplicaSet",
                "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
              },
              "min_replicas": "2",
              "variable_list": [],
              "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
            }
          ],
          "description": "",
          "action_list": [
            {
              "description": "System action for creating an application",
              "type": "system",
              "uuid": "add06d3e-6f70-441a-bdf8-390409139a6c",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_profile",
                      "name": "Nutanix",
                      "uuid": "b8641418-f0cd-4eef-8189-a6eaa79615be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__DE_Provision_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                        "uuid": "d949711b-e118-4b37-aba2-e02d650729d4"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__DE_Provision_d7089f69_f4e4_4bae_871f_db099bd166a9",
                        "uuid": "1d885ca2-973a-4551-94b3-f6d677ddf19a"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__DE_Provision_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                        "uuid": "73ff63f0-58a6-4225-9afd-083e9d9affd9"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__create_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "9abcb471-d1cb-447c-82b6-a0674fa2e964"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__create_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "c45e834c-7d1f-411f-b495-fde59921fc29"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__create_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "76bbdfa3-488e-4bd0-82d8-83e0a10fb0cc"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__install_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                        "uuid": "c66c34ab-3b6f-4b8b-abf7-66a09571ba00"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__install_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                        "uuid": "22a4c904-ad3e-4d36-9a7d-bf82dbab0017"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__create_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "82c498fe-d4e2-4e1f-8118-f8cd48a6c376"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__create_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "acace118-36bb-4ad3-b543-e7c56b5088f1"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "2841c329-f32d-4787-af77-83caac87ee4c"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "c12f2875-e294-4d13-8c70-9bcf2ccfaada"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__install_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                        "uuid": "be303d0e-e935-4675-a282-5ff4e38f96e2"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__create_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "0dcb12fe-a847-4a28-beb3-ce8434a9ba81"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "c5ea04f5-323e-4fe8-aeaf-c06e3e931407"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Application_b8641418_f0cd_4eef_8189_a6eaa79615be",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "0dcb12fe-a847-4a28-beb3-ce8434a9ba81"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "54ec1a43-987f-42de-9b74-6b48be87fe05",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "c5ea04f5-323e-4fe8-aeaf-c06e3e931407"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__install_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "be303d0e-e935-4675-a282-5ff4e38f96e2"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "a6d5ad54-5c2f-414e-bdf4-eead2ab977fa",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "0dcb12fe-a847-4a28-beb3-ce8434a9ba81"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Provision_d7089f69_f4e4_4bae_871f_db099bd166a9",
                            "uuid": "1d885ca2-973a-4551-94b3-f6d677ddf19a"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "45be70ea-2508-476c-9a70-4bc8098ec339",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "76bbdfa3-488e-4bd0-82d8-83e0a10fb0cc"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "76bbdfa3-488e-4bd0-82d8-83e0a10fb0cc"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "0ca0aefc-547d-43c3-9f56-d9d61dbb563e",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__install_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "be303d0e-e935-4675-a282-5ff4e38f96e2"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "acace118-36bb-4ad3-b543-e7c56b5088f1"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "99eb2a73-b62a-4fc5-8dea-afa74d769a41",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "2841c329-f32d-4787-af77-83caac87ee4c"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__install_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                            "uuid": "22a4c904-ad3e-4d36-9a7d-bf82dbab0017"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "d8d92330-351d-46aa-95b6-8f7d74394692",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "acace118-36bb-4ad3-b543-e7c56b5088f1"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Provision_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                            "uuid": "d949711b-e118-4b37-aba2-e02d650729d4"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "fb47f301-8519-4ada-a0ef-b89d708f8d0d",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "c45e834c-7d1f-411f-b495-fde59921fc29"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "c45e834c-7d1f-411f-b495-fde59921fc29"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "354d75c0-d83e-4ac1-a473-780780eda9eb",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__install_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                            "uuid": "22a4c904-ad3e-4d36-9a7d-bf82dbab0017"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "82c498fe-d4e2-4e1f-8118-f8cd48a6c376"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "e6bc4406-4057-4887-b527-b5c1fa3c72e9",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "c12f2875-e294-4d13-8c70-9bcf2ccfaada"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__install_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                            "uuid": "c66c34ab-3b6f-4b8b-abf7-66a09571ba00"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "05adfe4a-5447-4432-9043-3c5a166a0700",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "82c498fe-d4e2-4e1f-8118-f8cd48a6c376"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Provision_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                            "uuid": "73ff63f0-58a6-4225-9afd-083e9d9affd9"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "a52dbb2b-ac80-4fcc-a014-452ac69123e2",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "9abcb471-d1cb-447c-82b6-a0674fa2e964"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "9abcb471-d1cb-447c-82b6-a0674fa2e964"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "fc76cd45-febb-4e87-8079-636a4eb16705",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__install_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                            "uuid": "c66c34ab-3b6f-4b8b-abf7-66a09571ba00"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "2841c329-f32d-4787-af77-83caac87ee4c"
                          },
                          "edge_type": "dependency",
                          "type": "",
                          "uuid": "65febb75-8eb9-432c-9806-809be1d663cc",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__install_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "be303d0e-e935-4675-a282-5ff4e38f96e2"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "c12f2875-e294-4d13-8c70-9bcf2ccfaada"
                          },
                          "edge_type": "dependency",
                          "type": "",
                          "uuid": "4a654060-9c83-48ce-a132-72bfb8ca44de",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "0dcb12fe-a847-4a28-beb3-ce8434a9ba81"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "95a4707e-c2d1-4db1-aeb2-08209a5075ba"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_blueprint_deployment",
                      "name": "e0823432_deployment",
                      "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__DE_Provision_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "CREATE_DEPLOYMENT_ELEMENT",
                    "variable_list": [],
                    "uuid": "d949711b-e118-4b37-aba2-e02d650729d4"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_blueprint_deployment",
                      "name": "9b1e9738_deployment",
                      "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__DE_Provision_d7089f69_f4e4_4bae_871f_db099bd166a9",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "CREATE_DEPLOYMENT_ELEMENT",
                    "variable_list": [],
                    "uuid": "1d885ca2-973a-4551-94b3-f6d677ddf19a"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_blueprint_deployment",
                      "name": "871434ae_deployment",
                      "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__DE_Provision_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "CREATE_DEPLOYMENT_ELEMENT",
                    "variable_list": [],
                    "uuid": "73ff63f0-58a6-4225-9afd-083e9d9affd9"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__create_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "52b73e8a-27bd-41b3-8a7c-d5822b971228"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "9abcb471-d1cb-447c-82b6-a0674fa2e964"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__create_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "16c52300-6568-4f9d-8abf-7fad590a18ac"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "c45e834c-7d1f-411f-b495-fde59921fc29"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__create_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "deaf522d-a578-48f2-b281-e446ee769188"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "76bbdfa3-488e-4bd0-82d8-83e0a10fb0cc"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVReplicaSetPackage",
                      "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__install_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "871434ae_deployment",
                        "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                        "uuid": "015de7d5-5fe6-4d2a-82cc-2e08ad07cb68"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "c66c34ab-3b6f-4b8b-abf7-66a09571ba00"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVConfigSetPackage",
                      "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__install_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "e0823432_deployment",
                        "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                        "uuid": "74248961-1408-4b07-8401-9f96b80bb049"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "22a4c904-ad3e-4d36-9a7d-bf82dbab0017"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__create_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "871434ae_deployment",
                        "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "e984fac1_runbook",
                        "uuid": "c4094263-6a43-46f6-9b5e-335b65860978"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "82c498fe-d4e2-4e1f-8118-f8cd48a6c376"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__create_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "e0823432_deployment",
                        "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "e8608054_runbook",
                        "uuid": "5dd0f77d-1c9e-4bbb-9d61-50c834876150"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "acace118-36bb-4ad3-b543-e7c56b5088f1"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "e0823432_deployment",
                        "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "d04295dc_runbook",
                        "uuid": "b4ac0257-a074-4e37-9f8b-9df8a30127f1"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "2841c329-f32d-4787-af77-83caac87ee4c"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "871434ae_deployment",
                        "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "6286b364_runbook",
                        "uuid": "b414ddcc-2221-4078-bac1-8c739a480a49"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "c12f2875-e294-4d13-8c70-9bcf2ccfaada"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVRouterPackage",
                      "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__install_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "9b1e9738_deployment",
                        "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                        "uuid": "f7c3e8ac-cfcc-4a79-b55f-c824b6c6dac7"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "be303d0e-e935-4675-a282-5ff4e38f96e2"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__create_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "9b1e9738_deployment",
                        "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "b9357b85_runbook",
                        "uuid": "c05fe841-6938-42a8-88c9-7f33e6d7d9f4"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "0dcb12fe-a847-4a28-beb3-ce8434a9ba81"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "9b1e9738_deployment",
                        "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "d9638df2_runbook",
                        "uuid": "5c819c37-f08f-442e-af24-e72806300a94"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "c5ea04f5-323e-4fe8-aeaf-c06e3e931407"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Application_b8641418_f0cd_4eef_8189_a6eaa79615be",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Application_b8641418_f0cd_4eef_8189_a6eaa79615be",
                  "uuid": "95a4707e-c2d1-4db1-aeb2-08209a5075ba"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "f47c6758-dded-4aee-900f-efa0339433fe"
              },
              "message_list": [],
              "name": "action_create"
            },
            {
              "description": "System action for starting an application",
              "type": "system",
              "uuid": "3d29a4fb-35bd-4289-b3c9-4009dbef216e",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_profile",
                      "name": "Nutanix",
                      "uuid": "b8641418-f0cd-4eef-8189-a6eaa79615be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__start_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "032a7a0e-f32c-483c-87c1-e1d6a0d1d9a2"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__start_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "7cca07b1-fae7-47d6-b706-29e90399bf51"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__start_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "9aa58ffa-3805-4a8b-bec6-959e39e763c0"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "377aef46-74e1-4d22-912d-e1edcfbf864a"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "d7d759f6-612b-4bcd-89e0-8d7447850d11"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "c35c1c46-7404-4800-9a6f-e37145b04a6c"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Application_b8641418_f0cd_4eef_8189_a6eaa79615be",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "032a7a0e-f32c-483c-87c1-e1d6a0d1d9a2"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "2f6b5763-664d-473a-a501-84b92f2a4f25",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "c35c1c46-7404-4800-9a6f-e37145b04a6c"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "7cca07b1-fae7-47d6-b706-29e90399bf51"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "ff1f4287-980e-4135-a904-fd694ce4e384",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "d7d759f6-612b-4bcd-89e0-8d7447850d11"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "9aa58ffa-3805-4a8b-bec6-959e39e763c0"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "0f333d78-c5cf-4b01-980f-40766c96558b",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "377aef46-74e1-4d22-912d-e1edcfbf864a"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "377aef46-74e1-4d22-912d-e1edcfbf864a"
                          },
                          "edge_type": "create_action_edge",
                          "type": "",
                          "uuid": "99052fbd-da7c-4432-a145-f02f87915591",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "c35c1c46-7404-4800-9a6f-e37145b04a6c"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "cc03a56c-9bd4-4939-969a-2d2ccb7f3643"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__start_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "5cdcc37b-17ca-49d7-a065-d0e8824c9176"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "032a7a0e-f32c-483c-87c1-e1d6a0d1d9a2"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__start_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "3cf91285-df41-4ae8-9a82-d1d1fae9de0e"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "7cca07b1-fae7-47d6-b706-29e90399bf51"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__start_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "39d98cd4-5475-46b0-87cf-6fa414cba73d"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "9aa58ffa-3805-4a8b-bec6-959e39e763c0"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "871434ae_deployment",
                        "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "6286b364_runbook",
                        "uuid": "b414ddcc-2221-4078-bac1-8c739a480a49"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "377aef46-74e1-4d22-912d-e1edcfbf864a"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "e0823432_deployment",
                        "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "d04295dc_runbook",
                        "uuid": "b4ac0257-a074-4e37-9f8b-9df8a30127f1"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "d7d759f6-612b-4bcd-89e0-8d7447850d11"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "9b1e9738_deployment",
                        "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "d9638df2_runbook",
                        "uuid": "5c819c37-f08f-442e-af24-e72806300a94"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "c35c1c46-7404-4800-9a6f-e37145b04a6c"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Application_b8641418_f0cd_4eef_8189_a6eaa79615be",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Application_b8641418_f0cd_4eef_8189_a6eaa79615be",
                  "uuid": "cc03a56c-9bd4-4939-969a-2d2ccb7f3643"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "067a7b2d-7dfd-4634-b4fc-c8fb05e7200d"
              },
              "message_list": [],
              "name": "action_start"
            },
            {
              "description": "System action for stopping an application",
              "type": "system",
              "uuid": "046eda3c-4537-41ba-94b2-a565ac8e32c0",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_profile",
                      "name": "Nutanix",
                      "uuid": "b8641418-f0cd-4eef-8189-a6eaa79615be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "cfdf620e-8ddc-4650-bf0c-0b8754ebce87"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "abfad83f-ff2e-4ec3-b1de-07f0f3777da1"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__stop_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "7d051a7d-27bb-4978-822b-0f645b2af346"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "8c5c66cf-d936-4508-b769-eab15a860063"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__stop_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "e0959135-4c67-4999-8c92-12657cf95d7f"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__stop_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "d1f22030-edaa-479a-8d88-fa851108cf55"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Application_b8641418_f0cd_4eef_8189_a6eaa79615be",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "cfdf620e-8ddc-4650-bf0c-0b8754ebce87"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "cddcb542-c708-44ce-90e3-58e2876a1540",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "7d051a7d-27bb-4978-822b-0f645b2af346"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "abfad83f-ff2e-4ec3-b1de-07f0f3777da1"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "dd847ac5-640a-4e44-896a-f6c03699b681",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "e0959135-4c67-4999-8c92-12657cf95d7f"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "8c5c66cf-d936-4508-b769-eab15a860063"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "97bad1fc-2d7f-49e6-9524-696e31415316",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "d1f22030-edaa-479a-8d88-fa851108cf55"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "cfdf620e-8ddc-4650-bf0c-0b8754ebce87"
                          },
                          "edge_type": "create_action_edge",
                          "type": "",
                          "uuid": "73035b98-bca7-479a-9384-b835b1da824b",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "8c5c66cf-d936-4508-b769-eab15a860063"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "89ef21cc-d603-4115-bf28-acd06b37adcc"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "9b1e9738_deployment",
                        "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "507a01b5_runbook",
                        "uuid": "3496374f-c76e-4ba7-bc72-53e1a0475fe3"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "cfdf620e-8ddc-4650-bf0c-0b8754ebce87"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "e0823432_deployment",
                        "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "c93943b8_runbook",
                        "uuid": "c2ba6f77-45ff-4d0c-80bc-6a13a88bac4e"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "abfad83f-ff2e-4ec3-b1de-07f0f3777da1"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__stop_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "b1596526-1c86-439a-bc55-2c21dc7bd5f4"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "7d051a7d-27bb-4978-822b-0f645b2af346"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "871434ae_deployment",
                        "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "ee3949e3_runbook",
                        "uuid": "706d1aab-530f-452d-8b33-15bfbdb2fdd7"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "8c5c66cf-d936-4508-b769-eab15a860063"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__stop_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "b4df5ceb-d327-4062-9d7b-39e0a1b5981a"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "e0959135-4c67-4999-8c92-12657cf95d7f"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__stop_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "d71665d8-30af-433f-a178-34a2182ff5a3"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "d1f22030-edaa-479a-8d88-fa851108cf55"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Application_b8641418_f0cd_4eef_8189_a6eaa79615be",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Application_b8641418_f0cd_4eef_8189_a6eaa79615be",
                  "uuid": "89ef21cc-d603-4115-bf28-acd06b37adcc"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "e6135c98-c367-43a1-b7b3-b4475ad713f3"
              },
              "message_list": [],
              "name": "action_stop"
            },
            {
              "description": "System action for deleting an application. Deletes physical machines as well",
              "type": "system",
              "uuid": "bedc203a-5df6-4dc9-a4f1-be26fc141242",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_profile",
                      "name": "Nutanix",
                      "uuid": "b8641418-f0cd-4eef-8189-a6eaa79615be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "0806375b-b883-4610-9f3a-9311c9efb9c8"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "12d5a5db-d103-41cd-afab-ba44cb36b676"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "f3385d2e-384b-43a8-a7f1-04411bfdbc58"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__uninstall_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                        "uuid": "ef076f70-4d78-429e-95e6-8b57dfdbdb11"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "5d745d10-b121-4e7d-b2fb-dc3af5cca315"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "a4450bc6-6e4a-4210-a042-95e45fad5dfb"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "7096a327-6320-4230-a59e-1301daac7f82"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__uninstall_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                        "uuid": "6e21de21-f228-4fd9-b46f-d899af88c9cd"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "3fd07011-68fb-4ddd-ba01-a247eec5cbce"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__DE_Delete_d7089f69_f4e4_4bae_871f_db099bd166a9",
                        "uuid": "a36d3e3d-a27b-4160-9782-a634fb17cb05"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__uninstall_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                        "uuid": "a60dfed6-4c4d-4cfc-a010-b3df6dc3ff7d"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "1bdbb526-d221-4acf-a612-e9d87792acf0"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "ffc0435c-c871-407a-baa7-91835de7cba1"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__DE_Delete_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                        "uuid": "8ed7a110-cf80-4fd2-a455-4dcf7d65e624"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__DE_Delete_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                        "uuid": "76e21d56-dfdd-4b8f-8144-c613549ff425"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Application_b8641418_f0cd_4eef_8189_a6eaa79615be",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "0806375b-b883-4610-9f3a-9311c9efb9c8"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "6c646c7d-e203-4cf1-a0ec-7e5c0617861e",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "12d5a5db-d103-41cd-afab-ba44cb36b676"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "12d5a5db-d103-41cd-afab-ba44cb36b676"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "ede9fec2-160f-4e1c-a390-bfc44dd1b97a",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__uninstall_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "ef076f70-4d78-429e-95e6-8b57dfdbdb11"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__uninstall_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "ef076f70-4d78-429e-95e6-8b57dfdbdb11"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "99c88f40-772f-470f-9510-7099e82bba9a",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "7096a327-6320-4230-a59e-1301daac7f82"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "7096a327-6320-4230-a59e-1301daac7f82"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "96c3ff91-dedc-4c4e-80ca-ecc99e7606a1",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Delete_d7089f69_f4e4_4bae_871f_db099bd166a9",
                            "uuid": "a36d3e3d-a27b-4160-9782-a634fb17cb05"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "a4450bc6-6e4a-4210-a042-95e45fad5dfb"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "08fc8093-59b2-4f57-a0be-5cb32ab27857",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "3fd07011-68fb-4ddd-ba01-a247eec5cbce"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "3fd07011-68fb-4ddd-ba01-a247eec5cbce"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "5a2e2635-f821-48f6-9b5e-1c874c262e3e",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__uninstall_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                            "uuid": "a60dfed6-4c4d-4cfc-a010-b3df6dc3ff7d"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "ffc0435c-c871-407a-baa7-91835de7cba1"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "a0e4cc18-e52b-4f4f-b45b-290bceb65baa",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Delete_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                            "uuid": "76e21d56-dfdd-4b8f-8144-c613549ff425"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__uninstall_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                            "uuid": "a60dfed6-4c4d-4cfc-a010-b3df6dc3ff7d"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "33f7604b-eaa8-48b5-bb3e-00e8c57a3f26",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "ffc0435c-c871-407a-baa7-91835de7cba1"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "f3385d2e-384b-43a8-a7f1-04411bfdbc58"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "d34c1a35-347b-45a4-b773-c3d4b11dace7",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "5d745d10-b121-4e7d-b2fb-dc3af5cca315"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "5d745d10-b121-4e7d-b2fb-dc3af5cca315"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "28db2e81-5cf6-4d4e-abe0-46f3083e272a",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__uninstall_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                            "uuid": "6e21de21-f228-4fd9-b46f-d899af88c9cd"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "1bdbb526-d221-4acf-a612-e9d87792acf0"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "b349bc99-1ea0-4325-adfb-b90534979db9",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Delete_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                            "uuid": "8ed7a110-cf80-4fd2-a455-4dcf7d65e624"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__uninstall_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                            "uuid": "6e21de21-f228-4fd9-b46f-d899af88c9cd"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "c31582db-0472-4b2d-b072-1067a023b2e3",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "1bdbb526-d221-4acf-a612-e9d87792acf0"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__uninstall_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "ef076f70-4d78-429e-95e6-8b57dfdbdb11"
                          },
                          "edge_type": "create_action_edge",
                          "type": "",
                          "uuid": "183e05c6-b02c-42c0-978e-efd76b86ccca",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "a4450bc6-6e4a-4210-a042-95e45fad5dfb"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "12d5a5db-d103-41cd-afab-ba44cb36b676"
                          },
                          "edge_type": "create_action_edge",
                          "type": "",
                          "uuid": "a555ffca-f1ec-4dcd-923b-f08ec0cc608f",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "f3385d2e-384b-43a8-a7f1-04411bfdbc58"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "92498a05-6cf9-4015-a83f-40585e89da79"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "9b1e9738_deployment",
                        "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "507a01b5_runbook",
                        "uuid": "3496374f-c76e-4ba7-bc72-53e1a0475fe3"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "0806375b-b883-4610-9f3a-9311c9efb9c8"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "9b1e9738_deployment",
                        "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "10af4ae1_runbook",
                        "uuid": "c6f4df33-07f7-4a79-b87b-7fe3ec243b48"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "12d5a5db-d103-41cd-afab-ba44cb36b676"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "871434ae_deployment",
                        "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "ee3949e3_runbook",
                        "uuid": "706d1aab-530f-452d-8b33-15bfbdb2fdd7"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "f3385d2e-384b-43a8-a7f1-04411bfdbc58"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVRouterPackage",
                      "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__uninstall_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "9b1e9738_deployment",
                        "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                        "uuid": "48b7c968-32a8-48d9-8bfa-9ba1f9a4da1e"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "ef076f70-4d78-429e-95e6-8b57dfdbdb11"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "871434ae_deployment",
                        "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "96df4d94_runbook",
                        "uuid": "8d242223-afda-4c13-b35f-b9dff6717e5e"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "5d745d10-b121-4e7d-b2fb-dc3af5cca315"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "e0823432_deployment",
                        "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "c93943b8_runbook",
                        "uuid": "c2ba6f77-45ff-4d0c-80bc-6a13a88bac4e"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "a4450bc6-6e4a-4210-a042-95e45fad5dfb"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "39f030aa-de7a-4bd0-b4bc-8ca45308b4cf"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "7096a327-6320-4230-a59e-1301daac7f82"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVReplicaSetPackage",
                      "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__uninstall_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "871434ae_deployment",
                        "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                        "uuid": "7a4f2ebd-ee07-4fec-973d-0a284ae72425"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "6e21de21-f228-4fd9-b46f-d899af88c9cd"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "e0823432_deployment",
                        "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "fd485dc4_runbook",
                        "uuid": "bd261c01-2c76-46fd-a615-846ed9b187b1"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "3fd07011-68fb-4ddd-ba01-a247eec5cbce"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_blueprint_deployment",
                      "name": "9b1e9738_deployment",
                      "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__DE_Delete_d7089f69_f4e4_4bae_871f_db099bd166a9",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DELETE_DEPLOYMENT_ELEMENT",
                    "variable_list": [],
                    "uuid": "a36d3e3d-a27b-4160-9782-a634fb17cb05"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVConfigSetPackage",
                      "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__uninstall_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "e0823432_deployment",
                        "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                        "uuid": "30e18a00-af7c-4abe-b6e9-5d05a611972a"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "a60dfed6-4c4d-4cfc-a010-b3df6dc3ff7d"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "c45c07c0-a1fe-4e10-a294-be3c54abb14a"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "1bdbb526-d221-4acf-a612-e9d87792acf0"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "4d2b8fca-dccf-4d06-93b9-eb6abbaa71de"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "ffc0435c-c871-407a-baa7-91835de7cba1"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_blueprint_deployment",
                      "name": "871434ae_deployment",
                      "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__DE_Delete_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DELETE_DEPLOYMENT_ELEMENT",
                    "variable_list": [],
                    "uuid": "8ed7a110-cf80-4fd2-a455-4dcf7d65e624"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_blueprint_deployment",
                      "name": "e0823432_deployment",
                      "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__DE_Delete_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DELETE_DEPLOYMENT_ELEMENT",
                    "variable_list": [],
                    "uuid": "76e21d56-dfdd-4b8f-8144-c613549ff425"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Application_b8641418_f0cd_4eef_8189_a6eaa79615be",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Application_b8641418_f0cd_4eef_8189_a6eaa79615be",
                  "uuid": "92498a05-6cf9-4015-a83f-40585e89da79"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "a927dbda-6070-4c91-97a1-5915aa14edc6"
              },
              "message_list": [],
              "name": "action_delete"
            },
            {
              "description": "System action for deleting an application. Does not delete physical machines",
              "type": "system",
              "uuid": "1b845e26-ea98-4739-81a6-d4e09e2cd980",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_profile",
                      "name": "Nutanix",
                      "uuid": "b8641418-f0cd-4eef-8189-a6eaa79615be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__soft_delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "b8477158-ff6b-4a8b-b0ec-b1625c51f2a3"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Package_Element_Delete_1299fd60_5173_4b37_8f46_3374f51e45dc",
                        "uuid": "e262f9c4-7dbc-4631-95e0-9579da5a0c27"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__soft_delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "ad642679-6fab-48db-9576-2c1078231dd9"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__soft_delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "f4d03faf-a9be-4b0b-b3fb-5434d7c4c77b"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__soft_delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "20b5d95f-7f7b-4881-8f18-0db3c54de25b"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Package_Element_Delete_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                        "uuid": "dc33b651-2618-4bc4-a10d-02dbe8090a41"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__soft_delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "b7c0ae44-8688-4858-b718-adce2696525a"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__DE_Soft_Delete_d7089f69_f4e4_4bae_871f_db099bd166a9",
                        "uuid": "7827b2a4-527e-4adf-aba0-90c13e12995b"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Package_Element_Delete_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                        "uuid": "69a726e9-a34f-4417-a867-7731745cff88"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__DE_Soft_Delete_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                        "uuid": "0c255b4d-7e84-42c5-ab8a-de45704ffa0b"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__soft_delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "be8f4761-c399-44ba-baea-d58082ba71cd"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__DE_Soft_Delete_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                        "uuid": "ea6d9690-b309-4a68-998a-160bce5a3c0a"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Application_b8641418_f0cd_4eef_8189_a6eaa79615be",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "b8477158-ff6b-4a8b-b0ec-b1625c51f2a3"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "234d41b9-f74c-4215-b08f-7bc07b3bbf9c",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Package_Element_Delete_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "e262f9c4-7dbc-4631-95e0-9579da5a0c27"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Package_Element_Delete_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "e262f9c4-7dbc-4631-95e0-9579da5a0c27"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "24ff9e56-1ac6-4f30-b6ad-26fe49c1e69e",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "20b5d95f-7f7b-4881-8f18-0db3c54de25b"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                            "uuid": "20b5d95f-7f7b-4881-8f18-0db3c54de25b"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "d7fe41e2-63fc-41b7-a8c2-150b222a14d1",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Soft_Delete_d7089f69_f4e4_4bae_871f_db099bd166a9",
                            "uuid": "7827b2a4-527e-4adf-aba0-90c13e12995b"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "f4d03faf-a9be-4b0b-b3fb-5434d7c4c77b"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "51b35ecc-cf35-468a-8d8c-0b38d852f6a7",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Package_Element_Delete_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                            "uuid": "69a726e9-a34f-4417-a867-7731745cff88"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "be8f4761-c399-44ba-baea-d58082ba71cd"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "e70a030e-ac46-4fc3-ae75-5a8ba8395df4",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Soft_Delete_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                            "uuid": "ea6d9690-b309-4a68-998a-160bce5a3c0a"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Package_Element_Delete_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                            "uuid": "69a726e9-a34f-4417-a867-7731745cff88"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "4cc8432c-ac72-4a54-8483-0824b770da75",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                            "uuid": "be8f4761-c399-44ba-baea-d58082ba71cd"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "ad642679-6fab-48db-9576-2c1078231dd9"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "a34b156f-3c08-4db5-89e8-09adb3f46732",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Package_Element_Delete_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                            "uuid": "dc33b651-2618-4bc4-a10d-02dbe8090a41"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "b7c0ae44-8688-4858-b718-adce2696525a"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "9b6cb409-27c9-4b6f-a596-dd5e0b282739",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__DE_Soft_Delete_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                            "uuid": "0c255b4d-7e84-42c5-ab8a-de45704ffa0b"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Package_Element_Delete_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                            "uuid": "dc33b651-2618-4bc4-a10d-02dbe8090a41"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "6659f58d-afcf-404a-bda8-791748987922",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                            "uuid": "b7c0ae44-8688-4858-b718-adce2696525a"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Package_Element_Delete_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "e262f9c4-7dbc-4631-95e0-9579da5a0c27"
                          },
                          "edge_type": "create_action_edge",
                          "type": "",
                          "uuid": "6deccc09-b205-4845-8b20-aa18e9351dff",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "f4d03faf-a9be-4b0b-b3fb-5434d7c4c77b"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "b8477158-ff6b-4a8b-b0ec-b1625c51f2a3"
                          },
                          "edge_type": "create_action_edge",
                          "type": "",
                          "uuid": "6867e2d9-481b-4b58-ab63-93391ff15ca2",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "ad642679-6fab-48db-9576-2c1078231dd9"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "5e9ff701-d339-41ec-939d-027fc060e31c"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__soft_delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "9b1e9738_deployment",
                        "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "67c7551e-6768-46db-bde8-e28af739c02f"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "b8477158-ff6b-4a8b-b0ec-b1625c51f2a3"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVRouterPackage",
                      "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Package_Element_Delete_1299fd60_5173_4b37_8f46_3374f51e45dc",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "SOFT_DELETE_ELEMENT",
                    "variable_list": [],
                    "uuid": "e262f9c4-7dbc-4631-95e0-9579da5a0c27"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__soft_delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "871434ae_deployment",
                        "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "c0f5e1e4-bdb8-4981-baa5-7f01c4a753ad"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "ad642679-6fab-48db-9576-2c1078231dd9"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__soft_delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "deployment_reference": {
                        "kind": "app_blueprint_deployment",
                        "name": "e0823432_deployment",
                        "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                      },
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "ae786a91-e961-47bb-afe7-05a0b7244131"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "f4d03faf-a9be-4b0b-b3fb-5434d7c4c77b"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVRouter",
                      "uuid": "8fe35f32-09f5-450b-8e45-b2b3f4258e0e"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__soft_delete_CRb_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Substrate_8fe35f32_09f5_450b_8e45_b2b3f4258e0e",
                        "uuid": "50a06689-a733-4df5-910b-847260cca798"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "20b5d95f-7f7b-4881-8f18-0db3c54de25b"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVReplicaSetPackage",
                      "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Package_Element_Delete_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "SOFT_DELETE_ELEMENT",
                    "variable_list": [],
                    "uuid": "dc33b651-2618-4bc4-a10d-02dbe8090a41"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVReplicaSet",
                      "uuid": "b1f911d6-516e-4e95-aa73-2db7e9dddf83"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__soft_delete_CRb_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Substrate_b1f911d6_516e_4e95_aa73_2db7e9dddf83",
                        "uuid": "674a9acb-0ce8-484c-ac15-ec4e5db43ca8"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "b7c0ae44-8688-4858-b718-adce2696525a"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_blueprint_deployment",
                      "name": "9b1e9738_deployment",
                      "uuid": "d7089f69-f4e4-4bae-871f-db099bd166a9"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__DE_Soft_Delete_d7089f69_f4e4_4bae_871f_db099bd166a9",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "SOFT_DELETE_DEPLOYMENT_ELEMENT",
                    "variable_list": [],
                    "uuid": "7827b2a4-527e-4adf-aba0-90c13e12995b"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVConfigSetPackage",
                      "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Package_Element_Delete_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "SOFT_DELETE_ELEMENT",
                    "variable_list": [],
                    "uuid": "69a726e9-a34f-4417-a867-7731745cff88"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_blueprint_deployment",
                      "name": "871434ae_deployment",
                      "uuid": "d1638563-f8ed-43e2-83a8-5eca76d43d69"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__DE_Soft_Delete_d1638563_f8ed_43e2_83a8_5eca76d43d69",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "SOFT_DELETE_DEPLOYMENT_ELEMENT",
                    "variable_list": [],
                    "uuid": "0c255b4d-7e84-42c5-ab8a-de45704ffa0b"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_substrate",
                      "name": "AHVConfigSet",
                      "uuid": "e9069b03-7e28-4fed-b7df-7a75bd909478"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__soft_delete_CRb_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Substrate_e9069b03_7e28_4fed_b7df_7a75bd909478",
                        "uuid": "4bfd4ceb-6310-4fc9-b694-3568aff778bc"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "be8f4761-c399-44ba-baea-d58082ba71cd"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_blueprint_deployment",
                      "name": "e0823432_deployment",
                      "uuid": "8d78d84a-4b13-4a60-a7e5-e16eb0866d7c"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__DE_Soft_Delete_8d78d84a_4b13_4a60_a7e5_e16eb0866d7c",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "SOFT_DELETE_DEPLOYMENT_ELEMENT",
                    "variable_list": [],
                    "uuid": "ea6d9690-b309-4a68-998a-160bce5a3c0a"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Application_b8641418_f0cd_4eef_8189_a6eaa79615be",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Application_b8641418_f0cd_4eef_8189_a6eaa79615be",
                  "uuid": "5e9ff701-d339-41ec-939d-027fc060e31c"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "81e97b7c-10e7-44e3-8c0f-b9d6e99fafd4"
              },
              "message_list": [],
              "name": "action_soft_delete"
            }
          ],
          "name": "Nutanix",
          "state": "ACTIVE",
          "message_list": [],
          "dependency_list": [
            {
              "getter_resource_kind": "PackageCfg",
              "setter_resource_attr": "address",
              "setter_resource_kind": "ServiceCfg",
              "setter_resource_name": "Mongo_ConfigSet",
              "action_name": "action_create",
              "action_resource_name": "Nutanix",
              "getter_resource_name": "AHVRouterPackage",
              "action_resource_kind": "ApplicationCfg"
            },
            {
              "getter_resource_kind": "ServiceCfg",
              "setter_resource_attr": "address",
              "setter_resource_kind": "ServiceCfg",
              "setter_resource_name": "Mongo_ReplicaSet",
              "action_name": "action_create",
              "action_resource_name": "Nutanix",
              "getter_resource_name": "Mongo_Router",
              "action_resource_kind": "ApplicationCfg"
            },
            {
              "getter_resource_kind": "ServiceCfg",
              "setter_resource_attr": "address",
              "setter_resource_kind": "ServiceCfg",
              "setter_resource_name": "Mongo_ReplicaSet",
              "action_name": "action_start",
              "action_resource_name": "Nutanix",
              "getter_resource_name": "Mongo_Router",
              "action_resource_kind": "ApplicationCfg"
            }
          ],
          "variable_list": [
            {
              "val_type": "STRING",
              "description": "",
              "name": "DB_PATH",
              "type": "LOCAL",
              "value": "/opt/mongodb",
              "label": "",
              "state": "ACTIVE",
              "attrs": {
                "type": ""
              },
              "editables": {
                "value": true
              },
              "message_list": [],
              "uuid": "4725d1f6-5ac2-47e1-9c36-03861a4624f2"
            },
            {
              "val_type": "STRING",
              "description": "",
              "name": "NODES_PER_REPLICA_SET",
              "type": "LOCAL",
              "value": "2",
              "label": "",
              "state": "ACTIVE",
              "attrs": {
                "type": ""
              },
              "editables": {
                "value": true
              },
              "message_list": [],
              "uuid": "2f4f339b-a220-4eea-9c62-e57ce1e04411"
            },
            {
              "val_type": "STRING",
              "description": "",
              "name": "INSTANCE_PUBLIC_KEY",
              "type": "LOCAL",
              "value": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDMGAS/K0Mb/uwFXwD+hkjguy7VMZk2hpuhwPl9FUZwVBrURf/i9QMJ5/paPEZixu8VlRx7Inu4iun7rQfrnfeIYInmBwspXHYiTK3oHJAgZnrAHVEf1p6YaxLINlT1NI5yOAGPRWW6of8rBDBH1ObwU2+wcSx/1H0uIs3aZNLufr+Rh628ACxAum2Gt8AVRj6ua2BPFyt5VTdclyysAmeh1AiixNgOZXOz6y/i4TbzpY78I3isuKpxsUeXX6jxEMQol406jHDUF6njEOPIQG2zVZ3QJlTG9OlN+NiyZG9WkZz0VG/6M8ixxIHHI2dNwUbBFv2HUu+8X9LTLFq2O7KjX9Hp7uZKBAySHA3eKaKHIp2bZuU1bT5PRPkggngX86xg+T+OMNnutbAiMnRJ8+FvD5So+5TIx4b9GgxAxure3x2yRPT9lOiQOB+CVpJPxR0Rn9bOI+wiPnD0kAGvK/fHT+pqL4PM+hTnJtp9rrCRzIQApBx1263jEcYffhW2epZQRO+he5CMawFJ5TBe08om2AaDJ8GQdrpF6YA3W8DzHbmL3DPVVHdmqPLn10o+LX4gv5SdIIDVGdjKOc1BCnLTRmM28d5+sLDt/M+kvcQgf0y0yDjMVjGECZkt39hbm4ELMHzZtzYLmHNhBZxRqHeJ7qFTuv1kx88OW3Xc5mbBNQ== centos@nutanix.com",
              "label": "",
              "state": "ACTIVE",
              "attrs": {
                "type": ""
              },
              "editables": {
                "value": true
              },
              "message_list": [],
              "uuid": "6f6f1101-fcf5-44a5-ac41-af1505517836"
            }
          ],
          "uuid": "b8641418-f0cd-4eef-8189-a6eaa79615be"
        }
      ],
      "default_credential_local_reference": {
        "kind": "app_credential",
        "name": "CENTOS",
        "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
      },
      "package_definition_list": [
        {
          "description": "",
          "action_list": [
            {
              "description": "System action for installing an application",
              "type": "system",
              "uuid": "879473e5-5ba0-4658-b278-c42d050b0d0e",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVRouterPackage",
                      "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "PE_Install_1299fd60_5173_4b37_8f46_3374f51e45dc",
                        "uuid": "475bed06-1de7-40bc-86ee-70e8f4fc41ae"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "77bd316f-5071-4e96-a40c-ccce22c669ba"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVRouterPackage",
                      "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "PE_Install_1299fd60_5173_4b37_8f46_3374f51e45dc",
                    "state": "ACTIVE",
                    "attrs": {
                      "install_runbook": {
                        "task_definition_list": [
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVRouterPackage",
                              "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [
                              {
                                "kind": "app_task",
                                "name": "PackageInstallTask",
                                "uuid": "113c179b-0688-4171-a291-33f2cd38f03d"
                              }
                            ],
                            "name": "feb88e42_dag",
                            "state": "ACTIVE",
                            "attrs": {
                              "edges": [],
                              "type": ""
                            },
                            "timeout_secs": "",
                            "type": "DAG",
                            "variable_list": [],
                            "uuid": "9531ee5d-fa55-4966-9e6c-b74f68a7d087"
                          },
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVRouterPackage",
                              "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [],
                            "name": "PackageInstallTask",
                            "state": "ACTIVE",
                            "attrs": {
                              "exit_status": [],
                              "script": "#!/bin/bash\nset -ex\nif [[ -f /sys/kernel/mm/transparent_hugepage/enabled ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled\nfi\nif [[ -f /sys/kernel/mm/transparent_hugepage/defrag ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag\nfi\n\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/sysconfig/selinux\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config\nsudo setenforce 0\n\nVERSION=\"3.4.2\"\nDB_PATH=\"@@{DB_PATH}@@\"\nDB_PATH=${DB_PATH:=\"/opt/mongodb\"}\nCONFIG_SRV_IPS=\"@@{Mongo_ConfigSet.address}@@\"\n\nsudo mkdir -p ${DB_PATH}\n\n\nconfig_ips=$(echo \"${CONFIG_SRV_IPS}\" | sed 's/^,//' | sed 's/,$//' | sed 's/,/:37019,/g')\nconfig_ips=$config_ips\":37019\"\n\necho '[mongodb-org-3.4]\nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/\ngpgcheck=1\nenabled=1\ngpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc' | sudo tee /etc/yum.repos.d/mongodb-3.4.repo\n\n# Add mongodb repository\nsudo yum update -y\n\n# Install mongodb packages\nsudo yum install -y mongodb-org-${VERSION} mongodb-org-server-${VERSION} mongodb-org-shell-${VERSION} mongodb-org-mongos-${VERSION} mongodb-org-tools-${VERSION}\n\n# Make sure mongodb does not get updated automatically\necho 'exclude=mongodb-org*' | sudo tee -a /etc/yum.conf\n\n# Prepare the path for the databases\nsudo chown mongod:mongod ${DB_PATH}\n\n# Enable mongodb to listen for remote connections\nsudo sed -i 's/bindIp:/#bindIp:/g' /etc/mongod.conf\nsudo sed -i \"s#/var/lib/mongo#${DB_PATH}#g\" /etc/mongod.conf\n\nsudo -u mongod -b mongos --port 27017 --configdb configReplSet/$config_ips  --logpath /var/log/mongodb/mongos.log \n\nsleep 2\n",
                              "script_type": "sh",
                              "type": "",
                              "command_line_args": "",
                              "login_credential_local_reference": {
                                "kind": "app_credential",
                                "name": "CENTOS",
                                "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                              }
                            },
                            "timeout_secs": "",
                            "type": "EXEC",
                            "variable_list": [],
                            "uuid": "113c179b-0688-4171-a291-33f2cd38f03d"
                          }
                        ],
                        "description": "",
                        "name": "f2eb973b_runbook",
                        "state": "ACTIVE",
                        "main_task_local_reference": {
                          "kind": "app_task",
                          "name": "feb88e42_dag",
                          "uuid": "9531ee5d-fa55-4966-9e6c-b74f68a7d087"
                        },
                        "message_list": [],
                        "variable_list": [],
                        "uuid": "a66d9d78-1f0d-4ece-98db-95275eaafd97"
                      },
                      "type": "",
                      "uninstall_runbook": {
                        "task_definition_list": [
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVRouterPackage",
                              "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [
                              {
                                "kind": "app_task",
                                "name": "PackageUninstallTask",
                                "uuid": "71b7ee6c-bc1c-4530-9b98-b8c2d90d6409"
                              }
                            ],
                            "name": "db8d17a7_dag",
                            "state": "ACTIVE",
                            "attrs": {
                              "edges": [],
                              "type": ""
                            },
                            "timeout_secs": "",
                            "type": "DAG",
                            "variable_list": [],
                            "uuid": "bd9e24b2-b2c0-493d-8372-832a83ef38ab"
                          },
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVRouterPackage",
                              "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [],
                            "name": "PackageUninstallTask",
                            "state": "ACTIVE",
                            "attrs": {
                              "exit_status": [],
                              "script": "#!/bin/bash",
                              "script_type": "sh",
                              "type": "",
                              "command_line_args": "",
                              "login_credential_local_reference": {
                                "kind": "app_credential",
                                "name": "CENTOS",
                                "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                              }
                            },
                            "timeout_secs": "",
                            "type": "EXEC",
                            "variable_list": [],
                            "uuid": "71b7ee6c-bc1c-4530-9b98-b8c2d90d6409"
                          }
                        ],
                        "description": "",
                        "name": "cf93cd7c_runbook",
                        "state": "ACTIVE",
                        "main_task_local_reference": {
                          "kind": "app_task",
                          "name": "db8d17a7_dag",
                          "uuid": "bd9e24b2-b2c0-493d-8372-832a83ef38ab"
                        },
                        "message_list": [],
                        "variable_list": [],
                        "uuid": "23d4fdda-3dc3-4e1c-bdfa-e037375e16a4"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CUSTOM_PACKAGE_INSTALL",
                    "variable_list": [],
                    "uuid": "475bed06-1de7-40bc-86ee-70e8f4fc41ae"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                  "uuid": "77bd316f-5071-4e96-a40c-ccce22c669ba"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "f7c3e8ac-cfcc-4a79-b55f-c824b6c6dac7"
              },
              "message_list": [],
              "name": "action_install"
            },
            {
              "description": "System action for creating an application",
              "type": "system",
              "uuid": "68955513-a198-4252-95a5-c95fd2d4e88c",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVRouterPackage",
                      "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__install_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                        "uuid": "2259b2d6-c15f-41ed-9537-3e0c0e5d9dbf"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__create_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "1d6c5379-ef95-4296-84b2-bd13c98578fd"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "70df6b33-899f-460d-a646-f8fba3b3873b"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "1d6c5379-ef95-4296-84b2-bd13c98578fd"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "e378087f-ab50-442e-9018-6edb031c2b87",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "70df6b33-899f-460d-a646-f8fba3b3873b"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__install_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "2259b2d6-c15f-41ed-9537-3e0c0e5d9dbf"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "dcf3da83-604f-432a-944c-71025c0d2664",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "1d6c5379-ef95-4296-84b2-bd13c98578fd"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "18f1746a-d0ed-49af-a93b-38dc7beb7d3a"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVRouterPackage",
                      "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__install_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                        "uuid": "f7c3e8ac-cfcc-4a79-b55f-c824b6c6dac7"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "2259b2d6-c15f-41ed-9537-3e0c0e5d9dbf"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__create_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "b9357b85_runbook",
                        "uuid": "c05fe841-6938-42a8-88c9-7f33e6d7d9f4"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "1d6c5379-ef95-4296-84b2-bd13c98578fd"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "d9638df2_runbook",
                        "uuid": "5c819c37-f08f-442e-af24-e72806300a94"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "70df6b33-899f-460d-a646-f8fba3b3873b"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                  "uuid": "18f1746a-d0ed-49af-a93b-38dc7beb7d3a"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "d1abdec3-5c3f-4d8e-a9e3-76a7e01db2dd"
              },
              "message_list": [],
              "name": "action_create"
            },
            {
              "description": "System action for uninstalling an application",
              "type": "system",
              "uuid": "b320c09c-9301-4093-becd-9ce8814012b8",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVRouterPackage",
                      "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "PE_Uninstall_1299fd60_5173_4b37_8f46_3374f51e45dc",
                        "uuid": "3ff589a5-9d14-4f4f-a2df-b9699fde821f"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "f4e4ee79-5a03-492e-bd06-fe5949f8155e"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVRouterPackage",
                      "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "PE_Uninstall_1299fd60_5173_4b37_8f46_3374f51e45dc",
                    "state": "ACTIVE",
                    "attrs": {
                      "install_runbook": {
                        "task_definition_list": [
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVRouterPackage",
                              "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [
                              {
                                "kind": "app_task",
                                "name": "PackageInstallTask",
                                "uuid": "113c179b-0688-4171-a291-33f2cd38f03d"
                              }
                            ],
                            "name": "feb88e42_dag",
                            "state": "ACTIVE",
                            "attrs": {
                              "edges": [],
                              "type": ""
                            },
                            "timeout_secs": "",
                            "type": "DAG",
                            "variable_list": [],
                            "uuid": "9531ee5d-fa55-4966-9e6c-b74f68a7d087"
                          },
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVRouterPackage",
                              "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [],
                            "name": "PackageInstallTask",
                            "state": "ACTIVE",
                            "attrs": {
                              "exit_status": [],
                              "script": "#!/bin/bash\nset -ex\nif [[ -f /sys/kernel/mm/transparent_hugepage/enabled ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled\nfi\nif [[ -f /sys/kernel/mm/transparent_hugepage/defrag ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag\nfi\n\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/sysconfig/selinux\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config\nsudo setenforce 0\n\nVERSION=\"3.4.2\"\nDB_PATH=\"@@{DB_PATH}@@\"\nDB_PATH=${DB_PATH:=\"/opt/mongodb\"}\nCONFIG_SRV_IPS=\"@@{Mongo_ConfigSet.address}@@\"\n\nsudo mkdir -p ${DB_PATH}\n\n\nconfig_ips=$(echo \"${CONFIG_SRV_IPS}\" | sed 's/^,//' | sed 's/,$//' | sed 's/,/:37019,/g')\nconfig_ips=$config_ips\":37019\"\n\necho '[mongodb-org-3.4]\nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/\ngpgcheck=1\nenabled=1\ngpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc' | sudo tee /etc/yum.repos.d/mongodb-3.4.repo\n\n# Add mongodb repository\nsudo yum update -y\n\n# Install mongodb packages\nsudo yum install -y mongodb-org-${VERSION} mongodb-org-server-${VERSION} mongodb-org-shell-${VERSION} mongodb-org-mongos-${VERSION} mongodb-org-tools-${VERSION}\n\n# Make sure mongodb does not get updated automatically\necho 'exclude=mongodb-org*' | sudo tee -a /etc/yum.conf\n\n# Prepare the path for the databases\nsudo chown mongod:mongod ${DB_PATH}\n\n# Enable mongodb to listen for remote connections\nsudo sed -i 's/bindIp:/#bindIp:/g' /etc/mongod.conf\nsudo sed -i \"s#/var/lib/mongo#${DB_PATH}#g\" /etc/mongod.conf\n\nsudo -u mongod -b mongos --port 27017 --configdb configReplSet/$config_ips  --logpath /var/log/mongodb/mongos.log \n\nsleep 2\n",
                              "script_type": "sh",
                              "type": "",
                              "command_line_args": "",
                              "login_credential_local_reference": {
                                "kind": "app_credential",
                                "name": "CENTOS",
                                "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                              }
                            },
                            "timeout_secs": "",
                            "type": "EXEC",
                            "variable_list": [],
                            "uuid": "113c179b-0688-4171-a291-33f2cd38f03d"
                          }
                        ],
                        "description": "",
                        "name": "f2eb973b_runbook",
                        "state": "ACTIVE",
                        "main_task_local_reference": {
                          "kind": "app_task",
                          "name": "feb88e42_dag",
                          "uuid": "9531ee5d-fa55-4966-9e6c-b74f68a7d087"
                        },
                        "message_list": [],
                        "variable_list": [],
                        "uuid": "a66d9d78-1f0d-4ece-98db-95275eaafd97"
                      },
                      "type": "",
                      "uninstall_runbook": {
                        "task_definition_list": [
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVRouterPackage",
                              "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [
                              {
                                "kind": "app_task",
                                "name": "PackageUninstallTask",
                                "uuid": "71b7ee6c-bc1c-4530-9b98-b8c2d90d6409"
                              }
                            ],
                            "name": "db8d17a7_dag",
                            "state": "ACTIVE",
                            "attrs": {
                              "edges": [],
                              "type": ""
                            },
                            "timeout_secs": "",
                            "type": "DAG",
                            "variable_list": [],
                            "uuid": "bd9e24b2-b2c0-493d-8372-832a83ef38ab"
                          },
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVRouterPackage",
                              "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [],
                            "name": "PackageUninstallTask",
                            "state": "ACTIVE",
                            "attrs": {
                              "exit_status": [],
                              "script": "#!/bin/bash",
                              "script_type": "sh",
                              "type": "",
                              "command_line_args": "",
                              "login_credential_local_reference": {
                                "kind": "app_credential",
                                "name": "CENTOS",
                                "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                              }
                            },
                            "timeout_secs": "",
                            "type": "EXEC",
                            "variable_list": [],
                            "uuid": "71b7ee6c-bc1c-4530-9b98-b8c2d90d6409"
                          }
                        ],
                        "description": "",
                        "name": "cf93cd7c_runbook",
                        "state": "ACTIVE",
                        "main_task_local_reference": {
                          "kind": "app_task",
                          "name": "db8d17a7_dag",
                          "uuid": "bd9e24b2-b2c0-493d-8372-832a83ef38ab"
                        },
                        "message_list": [],
                        "variable_list": [],
                        "uuid": "23d4fdda-3dc3-4e1c-bdfa-e037375e16a4"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CUSTOM_PACKAGE_UNINSTALL",
                    "variable_list": [],
                    "uuid": "3ff589a5-9d14-4f4f-a2df-b9699fde821f"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                  "uuid": "f4e4ee79-5a03-492e-bd06-fe5949f8155e"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "48b7c968-32a8-48d9-8bfa-9ba1f9a4da1e"
              },
              "message_list": [],
              "name": "action_uninstall"
            },
            {
              "description": "System action for deleting an application. Deletes physical machines as well",
              "type": "system",
              "uuid": "c05521fc-be67-4b6f-a881-f252f96c9563",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVRouterPackage",
                      "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "c6dbd9ec-95e2-4eb3-a89e-700e80a110c7"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "b9eee373-28f3-4a00-bb12-2f87ad181f36"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__uninstall_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                        "uuid": "107cd6a2-40cf-456a-8700-4d722e8a572e"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "c6dbd9ec-95e2-4eb3-a89e-700e80a110c7"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "2cea7acd-e392-4faa-af3e-788d5a313c5a",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "b9eee373-28f3-4a00-bb12-2f87ad181f36"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "b9eee373-28f3-4a00-bb12-2f87ad181f36"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "9e735a9f-d15b-4cfb-9b4c-0c57906e8918",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__uninstall_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "107cd6a2-40cf-456a-8700-4d722e8a572e"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "3e8f4717-f040-4b09-98a7-0a033bc1409d"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "507a01b5_runbook",
                        "uuid": "3496374f-c76e-4ba7-bc72-53e1a0475fe3"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "c6dbd9ec-95e2-4eb3-a89e-700e80a110c7"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "10af4ae1_runbook",
                        "uuid": "c6f4df33-07f7-4a79-b87b-7fe3ec243b48"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "b9eee373-28f3-4a00-bb12-2f87ad181f36"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVRouterPackage",
                      "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__uninstall_CRb_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                        "uuid": "48b7c968-32a8-48d9-8bfa-9ba1f9a4da1e"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "107cd6a2-40cf-456a-8700-4d722e8a572e"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                  "uuid": "3e8f4717-f040-4b09-98a7-0a033bc1409d"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "2e2f04ff-5317-40b7-8ed6-14dc19ff9825"
              },
              "message_list": [],
              "name": "action_delete"
            },
            {
              "description": "System action for deleting an application. Does not delete physical machines",
              "type": "system",
              "uuid": "cc9c8435-d9e9-49ca-a819-636818faf703",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVRouterPackage",
                      "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__soft_delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "7df276dc-e0c4-4284-922b-b3c17c5c4a27"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Package_Element_Delete_1299fd60_5173_4b37_8f46_3374f51e45dc",
                        "uuid": "fd73a267-f7cf-40a5-ada4-270a5e1fa711"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                            "uuid": "7df276dc-e0c4-4284-922b-b3c17c5c4a27"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "dc7946d9-706d-44ba-ad20-df97d49b8274",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Package_Element_Delete_1299fd60_5173_4b37_8f46_3374f51e45dc",
                            "uuid": "fd73a267-f7cf-40a5-ada4-270a5e1fa711"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "e26f37df-6b6e-4ec3-b901-4a12d4a6c2ee"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__soft_delete_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "67c7551e-6768-46db-bde8-e28af739c02f"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "7df276dc-e0c4-4284-922b-b3c17c5c4a27"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVRouterPackage",
                      "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Package_Element_Delete_1299fd60_5173_4b37_8f46_3374f51e45dc",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "SOFT_DELETE_ELEMENT",
                    "variable_list": [],
                    "uuid": "fd73a267-f7cf-40a5-ada4-270a5e1fa711"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                  "uuid": "e26f37df-6b6e-4ec3-b901-4a12d4a6c2ee"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "b26f43e8-325d-4f13-b0eb-a403612009e2"
              },
              "message_list": [],
              "name": "action_soft_delete"
            },
            {
              "description": "System action for starting an application",
              "type": "system",
              "uuid": "ba93c1a2-3247-43cc-9346-6c10ec20f848",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVRouterPackage",
                      "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "e6c21f2e-61d1-4b58-8501-6eb67982601d"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "23c490a2-deef-48b8-8996-056f4aab189d"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__start_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "d9638df2_runbook",
                        "uuid": "5c819c37-f08f-442e-af24-e72806300a94"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "e6c21f2e-61d1-4b58-8501-6eb67982601d"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                  "uuid": "23c490a2-deef-48b8-8996-056f4aab189d"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "8d577986-a22f-4e29-8cbf-d8b0c41556dd"
              },
              "message_list": [],
              "name": "action_start"
            },
            {
              "description": "System action for stopping an application",
              "type": "system",
              "uuid": "813f2cf6-7392-4973-9923-a5a4a80e8d65",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVRouterPackage",
                      "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                        "uuid": "938bb814-b550-465c-9ff2-78240b45d815"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "37c3445e-c659-4233-b620-f44eee965524"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router",
                      "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__stop_CRb_Service_f2cc2e22_3838_44c4_89ba_a61d2e2acfbe",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "507a01b5_runbook",
                        "uuid": "3496374f-c76e-4ba7-bc72-53e1a0475fe3"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "938bb814-b550-465c-9ff2-78240b45d815"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Package_1299fd60_5173_4b37_8f46_3374f51e45dc",
                  "uuid": "37c3445e-c659-4233-b620-f44eee965524"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "8a5f0814-7ad0-46e6-9e1a-e0b76dc5c9f6"
              },
              "message_list": [],
              "name": "action_stop"
            }
          ],
          "type": "DEB",
          "service_local_reference_list": [
            {
              "kind": "app_service",
              "name": "Mongo_Router",
              "uuid": "f2cc2e22-3838-44c4-89ba-a61d2e2acfbe"
            }
          ],
          "name": "AHVRouterPackage",
          "state": "ACTIVE",
          "version": "",
          "editables": {},
          "message_list": [],
          "options": {
            "install_runbook": {
              "task_definition_list": [
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVRouterPackage",
                    "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                  },
                  "description": "",
                  "message_list": [],
                  "child_tasks_local_reference_list": [
                    {
                      "kind": "app_task",
                      "name": "PackageInstallTask",
                      "uuid": "113c179b-0688-4171-a291-33f2cd38f03d"
                    }
                  ],
                  "name": "feb88e42_dag",
                  "state": "ACTIVE",
                  "attrs": {
                    "edges": [],
                    "type": ""
                  },
                  "timeout_secs": "",
                  "type": "DAG",
                  "variable_list": [],
                  "uuid": "9531ee5d-fa55-4966-9e6c-b74f68a7d087"
                },
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVRouterPackage",
                    "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                  },
                  "description": "",
                  "message_list": [],
                  "child_tasks_local_reference_list": [],
                  "name": "PackageInstallTask",
                  "state": "ACTIVE",
                  "attrs": {
                    "exit_status": [],
                    "script": "#!/bin/bash\nset -ex\nif [[ -f /sys/kernel/mm/transparent_hugepage/enabled ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled\nfi\nif [[ -f /sys/kernel/mm/transparent_hugepage/defrag ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag\nfi\n\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/sysconfig/selinux\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config\nsudo setenforce 0\n\nVERSION=\"3.4.2\"\nDB_PATH=\"@@{DB_PATH}@@\"\nDB_PATH=${DB_PATH:=\"/opt/mongodb\"}\nCONFIG_SRV_IPS=\"@@{Mongo_ConfigSet.address}@@\"\n\nsudo mkdir -p ${DB_PATH}\n\n\nconfig_ips=$(echo \"${CONFIG_SRV_IPS}\" | sed 's/^,//' | sed 's/,$//' | sed 's/,/:37019,/g')\nconfig_ips=$config_ips\":37019\"\n\necho '[mongodb-org-3.4]\nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/\ngpgcheck=1\nenabled=1\ngpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc' | sudo tee /etc/yum.repos.d/mongodb-3.4.repo\n\n# Add mongodb repository\nsudo yum update -y\n\n# Install mongodb packages\nsudo yum install -y mongodb-org-${VERSION} mongodb-org-server-${VERSION} mongodb-org-shell-${VERSION} mongodb-org-mongos-${VERSION} mongodb-org-tools-${VERSION}\n\n# Make sure mongodb does not get updated automatically\necho 'exclude=mongodb-org*' | sudo tee -a /etc/yum.conf\n\n# Prepare the path for the databases\nsudo chown mongod:mongod ${DB_PATH}\n\n# Enable mongodb to listen for remote connections\nsudo sed -i 's/bindIp:/#bindIp:/g' /etc/mongod.conf\nsudo sed -i \"s#/var/lib/mongo#${DB_PATH}#g\" /etc/mongod.conf\n\nsudo -u mongod -b mongos --port 27017 --configdb configReplSet/$config_ips  --logpath /var/log/mongodb/mongos.log \n\nsleep 2\n",
                    "script_type": "sh",
                    "type": "",
                    "command_line_args": "",
                    "login_credential_local_reference": {
                      "kind": "app_credential",
                      "name": "CENTOS",
                      "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                    }
                  },
                  "timeout_secs": "",
                  "type": "EXEC",
                  "variable_list": [],
                  "uuid": "113c179b-0688-4171-a291-33f2cd38f03d"
                }
              ],
              "description": "",
              "name": "f2eb973b_runbook",
              "state": "ACTIVE",
              "main_task_local_reference": {
                "kind": "app_task",
                "name": "feb88e42_dag",
                "uuid": "9531ee5d-fa55-4966-9e6c-b74f68a7d087"
              },
              "message_list": [],
              "variable_list": [],
              "uuid": "a66d9d78-1f0d-4ece-98db-95275eaafd97"
            },
            "type": "",
            "uninstall_runbook": {
              "task_definition_list": [
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVRouterPackage",
                    "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                  },
                  "description": "",
                  "message_list": [],
                  "child_tasks_local_reference_list": [
                    {
                      "kind": "app_task",
                      "name": "PackageUninstallTask",
                      "uuid": "71b7ee6c-bc1c-4530-9b98-b8c2d90d6409"
                    }
                  ],
                  "name": "db8d17a7_dag",
                  "state": "ACTIVE",
                  "attrs": {
                    "edges": [],
                    "type": ""
                  },
                  "timeout_secs": "",
                  "type": "DAG",
                  "variable_list": [],
                  "uuid": "bd9e24b2-b2c0-493d-8372-832a83ef38ab"
                },
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVRouterPackage",
                    "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
                  },
                  "description": "",
                  "message_list": [],
                  "child_tasks_local_reference_list": [],
                  "name": "PackageUninstallTask",
                  "state": "ACTIVE",
                  "attrs": {
                    "exit_status": [],
                    "script": "#!/bin/bash",
                    "script_type": "sh",
                    "type": "",
                    "command_line_args": "",
                    "login_credential_local_reference": {
                      "kind": "app_credential",
                      "name": "CENTOS",
                      "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                    }
                  },
                  "timeout_secs": "",
                  "type": "EXEC",
                  "variable_list": [],
                  "uuid": "71b7ee6c-bc1c-4530-9b98-b8c2d90d6409"
                }
              ],
              "description": "",
              "name": "cf93cd7c_runbook",
              "state": "ACTIVE",
              "main_task_local_reference": {
                "kind": "app_task",
                "name": "db8d17a7_dag",
                "uuid": "bd9e24b2-b2c0-493d-8372-832a83ef38ab"
              },
              "message_list": [],
              "variable_list": [],
              "uuid": "23d4fdda-3dc3-4e1c-bdfa-e037375e16a4"
            }
          },
          "variable_list": [],
          "uuid": "1299fd60-5173-4b37-8f46-3374f51e45dc"
        },
        {
          "description": "",
          "action_list": [
            {
              "description": "System action for installing an application",
              "type": "system",
              "uuid": "9d501965-00bf-4c18-abc8-1ac90a33d589",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVConfigSetPackage",
                      "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "PE_Install_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                        "uuid": "b0029bbd-6189-4b44-8cda-5c8ff74233cd"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "9ff40895-84e9-4aa7-9b4a-ede1c7ae8c40"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVConfigSetPackage",
                      "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "PE_Install_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                    "state": "ACTIVE",
                    "attrs": {
                      "install_runbook": {
                        "task_definition_list": [
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVConfigSetPackage",
                              "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [
                              {
                                "kind": "app_task",
                                "name": "PackageInstallTask",
                                "uuid": "1415310a-d031-4815-a970-bb6e1910fc66"
                              }
                            ],
                            "name": "40b53b04_dag",
                            "state": "ACTIVE",
                            "attrs": {
                              "edges": [],
                              "type": ""
                            },
                            "timeout_secs": "",
                            "type": "DAG",
                            "variable_list": [],
                            "uuid": "0f7cceaf-0f2b-4fa7-8a44-55483209b3f8"
                          },
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVConfigSetPackage",
                              "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [],
                            "name": "PackageInstallTask",
                            "state": "ACTIVE",
                            "attrs": {
                              "exit_status": [],
                              "script": "#!/bin/bash\nset -ex\nif [[ -f /sys/kernel/mm/transparent_hugepage/enabled ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled\nfi\nif [[ -f /sys/kernel/mm/transparent_hugepage/defrag ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag\nfi\n\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/sysconfig/selinux\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config\nsudo setenforce 0\n\nVERSION=\"3.4.2\"\nDB_PATH=\"@@{DB_PATH}@@\"\nDB_PATH=${DB_PATH:=\"/opt/mongodb\"}\nCONFIG_SRV_IPS=\"@@{calm_array_address}@@\"\nHOST_IP=\"@@{address}@@\"\n\nsudo mkdir -p ${DB_PATH}\n\necho '[mongodb-org-3.4]\nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/\ngpgcheck=1\nenabled=1\ngpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc' | sudo tee /etc/yum.repos.d/mongodb-3.4.repo\n\n# Add mongodb repository\nsudo yum update -y\n\n# Install mongodb packages\nsudo yum install -y mongodb-org-${VERSION} mongodb-org-server-${VERSION} mongodb-org-shell-${VERSION} mongodb-org-mongos-${VERSION} mongodb-org-tools-${VERSION}\n\n# Make sure mongodb does not get updated automatically\necho 'exclude=mongodb-org*' | sudo tee -a /etc/yum.conf\n\n# Prepare the path for the databases\nsudo chown mongod:mongod ${DB_PATH}\n\n# Enable mongodb to listen for remote connections\nsudo sed -i 's/bindIp:/#bindIp:/g' /etc/mongod.conf\nsudo sed -i \"s#/var/lib/mongo#${DB_PATH}#g\" /etc/mongod.conf\nsudo sed -i 's/27017/37019/g' /etc/mongod.conf\nsudo sed -i 's/#sharding:/sharding:\\n  clusterRole: configsvr/g' /etc/mongod.conf\nsudo sed -i 's/#replication:/replication:\\n  replSetName: configReplSet/g' /etc/mongod.conf\n\n\nconfig_ips=$(echo \"${CONFIG_SRV_IPS}\" | sed 's/^,//' | sed 's/,$//' | tr \",\" \"\\n\")\nconfig_cnt=0\nconfig_str=\"\"\n\nfor config in $config_ips\ndo\n  config_str=$config_str\"{\\\"_id\\\": $config_cnt, host:\\\"$config:37019\\\"},\"\n  config_cnt=$(($config_cnt + 1))\ndone\nconfig_str=`echo $config_str | sed 's/,$//'`\njson=$(cat <<EOF\n{\n  \"_id\": \"configReplSet\",\n  \"members\": [\n    $config_str\n  ]\n}\nEOF\n)\n\nsudo systemctl restart mongod\nsleep 2\n\nsudo mongo --host ${HOST_IP} --port 37019 --eval \"rs.initiate( $json )\"\n\n",
                              "script_type": "sh",
                              "type": "",
                              "command_line_args": "",
                              "login_credential_local_reference": {
                                "kind": "app_credential",
                                "name": "CENTOS",
                                "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                              }
                            },
                            "timeout_secs": "",
                            "type": "EXEC",
                            "variable_list": [],
                            "uuid": "1415310a-d031-4815-a970-bb6e1910fc66"
                          }
                        ],
                        "description": "",
                        "name": "fb351d28_runbook",
                        "state": "ACTIVE",
                        "main_task_local_reference": {
                          "kind": "app_task",
                          "name": "40b53b04_dag",
                          "uuid": "0f7cceaf-0f2b-4fa7-8a44-55483209b3f8"
                        },
                        "message_list": [],
                        "variable_list": [],
                        "uuid": "a100d262-774b-4915-962a-4f0f0d56bb0b"
                      },
                      "type": "",
                      "uninstall_runbook": {
                        "task_definition_list": [
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVConfigSetPackage",
                              "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [
                              {
                                "kind": "app_task",
                                "name": "PackageUninstallTask",
                                "uuid": "b60e1c52-6c40-449f-8558-6b874e274607"
                              }
                            ],
                            "name": "87329188_dag",
                            "state": "ACTIVE",
                            "attrs": {
                              "edges": [],
                              "type": ""
                            },
                            "timeout_secs": "",
                            "type": "DAG",
                            "variable_list": [],
                            "uuid": "92908559-c693-4ed6-b4d4-c9a08046e202"
                          },
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVConfigSetPackage",
                              "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [],
                            "name": "PackageUninstallTask",
                            "state": "ACTIVE",
                            "attrs": {
                              "exit_status": [],
                              "script": "#!/bin/bash",
                              "script_type": "sh",
                              "type": "",
                              "command_line_args": "",
                              "login_credential_local_reference": {
                                "kind": "app_credential",
                                "name": "CENTOS",
                                "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                              }
                            },
                            "timeout_secs": "",
                            "type": "EXEC",
                            "variable_list": [],
                            "uuid": "b60e1c52-6c40-449f-8558-6b874e274607"
                          }
                        ],
                        "description": "",
                        "name": "d338a852_runbook",
                        "state": "ACTIVE",
                        "main_task_local_reference": {
                          "kind": "app_task",
                          "name": "87329188_dag",
                          "uuid": "92908559-c693-4ed6-b4d4-c9a08046e202"
                        },
                        "message_list": [],
                        "variable_list": [],
                        "uuid": "f0221c91-b30b-45f0-b58c-4e65c1022f93"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CUSTOM_PACKAGE_INSTALL",
                    "variable_list": [],
                    "uuid": "b0029bbd-6189-4b44-8cda-5c8ff74233cd"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                  "uuid": "9ff40895-84e9-4aa7-9b4a-ede1c7ae8c40"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "74248961-1408-4b07-8401-9f96b80bb049"
              },
              "message_list": [],
              "name": "action_install"
            },
            {
              "description": "System action for creating an application",
              "type": "system",
              "uuid": "d3cf8a71-6962-4608-85ab-6a3b6ab00da0",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVConfigSetPackage",
                      "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__install_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                        "uuid": "a577240e-2463-48cf-be54-5c27a4387935"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__create_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "36c50fa8-de69-43b4-b92c-138869d89aaf"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "b08dc076-1911-42ab-979c-9c8f309ca8bb"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "36c50fa8-de69-43b4-b92c-138869d89aaf"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "e329aada-7880-4898-ae5f-162267c99176",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "b08dc076-1911-42ab-979c-9c8f309ca8bb"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__install_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                            "uuid": "a577240e-2463-48cf-be54-5c27a4387935"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "036b7d06-5bc3-47f8-8298-beb3cb2aa7fc",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "36c50fa8-de69-43b4-b92c-138869d89aaf"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "03827f7c-6e0a-4918-9bf2-12261c7ae626"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVConfigSetPackage",
                      "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__install_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                        "uuid": "74248961-1408-4b07-8401-9f96b80bb049"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "a577240e-2463-48cf-be54-5c27a4387935"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__create_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "e8608054_runbook",
                        "uuid": "5dd0f77d-1c9e-4bbb-9d61-50c834876150"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "36c50fa8-de69-43b4-b92c-138869d89aaf"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "d04295dc_runbook",
                        "uuid": "b4ac0257-a074-4e37-9f8b-9df8a30127f1"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "b08dc076-1911-42ab-979c-9c8f309ca8bb"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                  "uuid": "03827f7c-6e0a-4918-9bf2-12261c7ae626"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "a88c7e79-6faa-4471-a453-9b46fcba2102"
              },
              "message_list": [],
              "name": "action_create"
            },
            {
              "description": "System action for uninstalling an application",
              "type": "system",
              "uuid": "6570b184-f0b2-4a79-9948-81e4ade83eb2",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVConfigSetPackage",
                      "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "PE_Uninstall_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                        "uuid": "eb37a1bd-6119-4fb9-9f76-27c48a3b684b"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "63507f85-a14e-445a-bc8d-86844793e90d"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVConfigSetPackage",
                      "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "PE_Uninstall_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                    "state": "ACTIVE",
                    "attrs": {
                      "install_runbook": {
                        "task_definition_list": [
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVConfigSetPackage",
                              "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [
                              {
                                "kind": "app_task",
                                "name": "PackageInstallTask",
                                "uuid": "1415310a-d031-4815-a970-bb6e1910fc66"
                              }
                            ],
                            "name": "40b53b04_dag",
                            "state": "ACTIVE",
                            "attrs": {
                              "edges": [],
                              "type": ""
                            },
                            "timeout_secs": "",
                            "type": "DAG",
                            "variable_list": [],
                            "uuid": "0f7cceaf-0f2b-4fa7-8a44-55483209b3f8"
                          },
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVConfigSetPackage",
                              "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [],
                            "name": "PackageInstallTask",
                            "state": "ACTIVE",
                            "attrs": {
                              "exit_status": [],
                              "script": "#!/bin/bash\nset -ex\nif [[ -f /sys/kernel/mm/transparent_hugepage/enabled ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled\nfi\nif [[ -f /sys/kernel/mm/transparent_hugepage/defrag ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag\nfi\n\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/sysconfig/selinux\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config\nsudo setenforce 0\n\nVERSION=\"3.4.2\"\nDB_PATH=\"@@{DB_PATH}@@\"\nDB_PATH=${DB_PATH:=\"/opt/mongodb\"}\nCONFIG_SRV_IPS=\"@@{calm_array_address}@@\"\nHOST_IP=\"@@{address}@@\"\n\nsudo mkdir -p ${DB_PATH}\n\necho '[mongodb-org-3.4]\nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/\ngpgcheck=1\nenabled=1\ngpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc' | sudo tee /etc/yum.repos.d/mongodb-3.4.repo\n\n# Add mongodb repository\nsudo yum update -y\n\n# Install mongodb packages\nsudo yum install -y mongodb-org-${VERSION} mongodb-org-server-${VERSION} mongodb-org-shell-${VERSION} mongodb-org-mongos-${VERSION} mongodb-org-tools-${VERSION}\n\n# Make sure mongodb does not get updated automatically\necho 'exclude=mongodb-org*' | sudo tee -a /etc/yum.conf\n\n# Prepare the path for the databases\nsudo chown mongod:mongod ${DB_PATH}\n\n# Enable mongodb to listen for remote connections\nsudo sed -i 's/bindIp:/#bindIp:/g' /etc/mongod.conf\nsudo sed -i \"s#/var/lib/mongo#${DB_PATH}#g\" /etc/mongod.conf\nsudo sed -i 's/27017/37019/g' /etc/mongod.conf\nsudo sed -i 's/#sharding:/sharding:\\n  clusterRole: configsvr/g' /etc/mongod.conf\nsudo sed -i 's/#replication:/replication:\\n  replSetName: configReplSet/g' /etc/mongod.conf\n\n\nconfig_ips=$(echo \"${CONFIG_SRV_IPS}\" | sed 's/^,//' | sed 's/,$//' | tr \",\" \"\\n\")\nconfig_cnt=0\nconfig_str=\"\"\n\nfor config in $config_ips\ndo\n  config_str=$config_str\"{\\\"_id\\\": $config_cnt, host:\\\"$config:37019\\\"},\"\n  config_cnt=$(($config_cnt + 1))\ndone\nconfig_str=`echo $config_str | sed 's/,$//'`\njson=$(cat <<EOF\n{\n  \"_id\": \"configReplSet\",\n  \"members\": [\n    $config_str\n  ]\n}\nEOF\n)\n\nsudo systemctl restart mongod\nsleep 2\n\nsudo mongo --host ${HOST_IP} --port 37019 --eval \"rs.initiate( $json )\"\n\n",
                              "script_type": "sh",
                              "type": "",
                              "command_line_args": "",
                              "login_credential_local_reference": {
                                "kind": "app_credential",
                                "name": "CENTOS",
                                "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                              }
                            },
                            "timeout_secs": "",
                            "type": "EXEC",
                            "variable_list": [],
                            "uuid": "1415310a-d031-4815-a970-bb6e1910fc66"
                          }
                        ],
                        "description": "",
                        "name": "fb351d28_runbook",
                        "state": "ACTIVE",
                        "main_task_local_reference": {
                          "kind": "app_task",
                          "name": "40b53b04_dag",
                          "uuid": "0f7cceaf-0f2b-4fa7-8a44-55483209b3f8"
                        },
                        "message_list": [],
                        "variable_list": [],
                        "uuid": "a100d262-774b-4915-962a-4f0f0d56bb0b"
                      },
                      "type": "",
                      "uninstall_runbook": {
                        "task_definition_list": [
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVConfigSetPackage",
                              "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [
                              {
                                "kind": "app_task",
                                "name": "PackageUninstallTask",
                                "uuid": "b60e1c52-6c40-449f-8558-6b874e274607"
                              }
                            ],
                            "name": "87329188_dag",
                            "state": "ACTIVE",
                            "attrs": {
                              "edges": [],
                              "type": ""
                            },
                            "timeout_secs": "",
                            "type": "DAG",
                            "variable_list": [],
                            "uuid": "92908559-c693-4ed6-b4d4-c9a08046e202"
                          },
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVConfigSetPackage",
                              "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [],
                            "name": "PackageUninstallTask",
                            "state": "ACTIVE",
                            "attrs": {
                              "exit_status": [],
                              "script": "#!/bin/bash",
                              "script_type": "sh",
                              "type": "",
                              "command_line_args": "",
                              "login_credential_local_reference": {
                                "kind": "app_credential",
                                "name": "CENTOS",
                                "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                              }
                            },
                            "timeout_secs": "",
                            "type": "EXEC",
                            "variable_list": [],
                            "uuid": "b60e1c52-6c40-449f-8558-6b874e274607"
                          }
                        ],
                        "description": "",
                        "name": "d338a852_runbook",
                        "state": "ACTIVE",
                        "main_task_local_reference": {
                          "kind": "app_task",
                          "name": "87329188_dag",
                          "uuid": "92908559-c693-4ed6-b4d4-c9a08046e202"
                        },
                        "message_list": [],
                        "variable_list": [],
                        "uuid": "f0221c91-b30b-45f0-b58c-4e65c1022f93"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CUSTOM_PACKAGE_UNINSTALL",
                    "variable_list": [],
                    "uuid": "eb37a1bd-6119-4fb9-9f76-27c48a3b684b"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                  "uuid": "63507f85-a14e-445a-bc8d-86844793e90d"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "30e18a00-af7c-4abe-b6e9-5d05a611972a"
              },
              "message_list": [],
              "name": "action_uninstall"
            },
            {
              "description": "System action for deleting an application. Deletes physical machines as well",
              "type": "system",
              "uuid": "a0dd1113-7c4e-418b-8fd3-f478f9a1dded",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVConfigSetPackage",
                      "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "61c29f19-c6df-4c04-b42e-bcd72709ba16"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "25a36c2a-1a29-4b66-b74b-72016e25381d"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__uninstall_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                        "uuid": "6c881962-8e51-40ea-995f-1f532286385d"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "61c29f19-c6df-4c04-b42e-bcd72709ba16"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "0f84355c-a1bb-4981-b2dd-f99aa8134305",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "25a36c2a-1a29-4b66-b74b-72016e25381d"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "25a36c2a-1a29-4b66-b74b-72016e25381d"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "2a381c5f-5f60-46d8-a213-3f24b4d58c3c",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__uninstall_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                            "uuid": "6c881962-8e51-40ea-995f-1f532286385d"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "b90ecd8e-24b1-45e3-9181-1f790736f305"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "c93943b8_runbook",
                        "uuid": "c2ba6f77-45ff-4d0c-80bc-6a13a88bac4e"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "61c29f19-c6df-4c04-b42e-bcd72709ba16"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "fd485dc4_runbook",
                        "uuid": "bd261c01-2c76-46fd-a615-846ed9b187b1"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "25a36c2a-1a29-4b66-b74b-72016e25381d"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVConfigSetPackage",
                      "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__uninstall_CRb_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                        "uuid": "30e18a00-af7c-4abe-b6e9-5d05a611972a"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "6c881962-8e51-40ea-995f-1f532286385d"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                  "uuid": "b90ecd8e-24b1-45e3-9181-1f790736f305"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "0f60b133-d135-4f77-b2e0-dbefa05d36e9"
              },
              "message_list": [],
              "name": "action_delete"
            },
            {
              "description": "System action for deleting an application. Does not delete physical machines",
              "type": "system",
              "uuid": "3c9758ec-a3dd-44be-abf5-815ab37abb57",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVConfigSetPackage",
                      "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__soft_delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "0188c12a-80bb-4ef6-9795-036ca79fdf47"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Package_Element_Delete_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                        "uuid": "c64de9e4-edbd-472a-b4d7-489c2bc9f26e"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                            "uuid": "0188c12a-80bb-4ef6-9795-036ca79fdf47"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "3f151ed3-85f4-4300-be1a-a356057045f2",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Package_Element_Delete_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                            "uuid": "c64de9e4-edbd-472a-b4d7-489c2bc9f26e"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "2ce4f8c3-6bec-4f79-8115-a60601d7cb48"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__soft_delete_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "ae786a91-e961-47bb-afe7-05a0b7244131"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "0188c12a-80bb-4ef6-9795-036ca79fdf47"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVConfigSetPackage",
                      "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Package_Element_Delete_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "SOFT_DELETE_ELEMENT",
                    "variable_list": [],
                    "uuid": "c64de9e4-edbd-472a-b4d7-489c2bc9f26e"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                  "uuid": "2ce4f8c3-6bec-4f79-8115-a60601d7cb48"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "c58a5c65-888a-4194-91eb-a9eb90a60faf"
              },
              "message_list": [],
              "name": "action_soft_delete"
            },
            {
              "description": "System action for starting an application",
              "type": "system",
              "uuid": "e1fd6b62-c70b-4adb-831a-093d55f817cf",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVConfigSetPackage",
                      "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "03074da5-0738-45e3-9b10-b4cba1fd6be0"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "7fd62249-095a-4422-a18a-424cb7a8fbce"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__start_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "d04295dc_runbook",
                        "uuid": "b4ac0257-a074-4e37-9f8b-9df8a30127f1"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "03074da5-0738-45e3-9b10-b4cba1fd6be0"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                  "uuid": "7fd62249-095a-4422-a18a-424cb7a8fbce"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "cdceb680-937c-4b9b-b3c5-2c092ccac164"
              },
              "message_list": [],
              "name": "action_start"
            },
            {
              "description": "System action for stopping an application",
              "type": "system",
              "uuid": "531fd8b8-74a4-47d4-9173-ab3ffdcd2b17",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVConfigSetPackage",
                      "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                        "uuid": "49cd7e62-26b5-4f3f-87cb-866a98f9565c"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "5b0ae332-9050-4d46-9411-8e60d14c8c13"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet",
                      "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__stop_CRb_Service_a8be05cc_2db1_43d6_a442_091235a69cd5",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "c93943b8_runbook",
                        "uuid": "c2ba6f77-45ff-4d0c-80bc-6a13a88bac4e"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "49cd7e62-26b5-4f3f-87cb-866a98f9565c"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Package_cf0e6f9c_040b_4757_a7ce_d79df73a00be",
                  "uuid": "5b0ae332-9050-4d46-9411-8e60d14c8c13"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "78832460-958b-4c99-b15a-12cb239041c1"
              },
              "message_list": [],
              "name": "action_stop"
            }
          ],
          "type": "DEB",
          "service_local_reference_list": [
            {
              "kind": "app_service",
              "name": "Mongo_ConfigSet",
              "uuid": "a8be05cc-2db1-43d6-a442-091235a69cd5"
            }
          ],
          "name": "AHVConfigSetPackage",
          "state": "ACTIVE",
          "version": "",
          "editables": {},
          "message_list": [],
          "options": {
            "install_runbook": {
              "task_definition_list": [
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVConfigSetPackage",
                    "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                  },
                  "description": "",
                  "message_list": [],
                  "child_tasks_local_reference_list": [
                    {
                      "kind": "app_task",
                      "name": "PackageInstallTask",
                      "uuid": "1415310a-d031-4815-a970-bb6e1910fc66"
                    }
                  ],
                  "name": "40b53b04_dag",
                  "state": "ACTIVE",
                  "attrs": {
                    "edges": [],
                    "type": ""
                  },
                  "timeout_secs": "",
                  "type": "DAG",
                  "variable_list": [],
                  "uuid": "0f7cceaf-0f2b-4fa7-8a44-55483209b3f8"
                },
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVConfigSetPackage",
                    "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                  },
                  "description": "",
                  "message_list": [],
                  "child_tasks_local_reference_list": [],
                  "name": "PackageInstallTask",
                  "state": "ACTIVE",
                  "attrs": {
                    "exit_status": [],
                    "script": "#!/bin/bash\nset -ex\nif [[ -f /sys/kernel/mm/transparent_hugepage/enabled ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled\nfi\nif [[ -f /sys/kernel/mm/transparent_hugepage/defrag ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag\nfi\n\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/sysconfig/selinux\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config\nsudo setenforce 0\n\nVERSION=\"3.4.2\"\nDB_PATH=\"@@{DB_PATH}@@\"\nDB_PATH=${DB_PATH:=\"/opt/mongodb\"}\nCONFIG_SRV_IPS=\"@@{calm_array_address}@@\"\nHOST_IP=\"@@{address}@@\"\n\nsudo mkdir -p ${DB_PATH}\n\necho '[mongodb-org-3.4]\nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/\ngpgcheck=1\nenabled=1\ngpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc' | sudo tee /etc/yum.repos.d/mongodb-3.4.repo\n\n# Add mongodb repository\nsudo yum update -y\n\n# Install mongodb packages\nsudo yum install -y mongodb-org-${VERSION} mongodb-org-server-${VERSION} mongodb-org-shell-${VERSION} mongodb-org-mongos-${VERSION} mongodb-org-tools-${VERSION}\n\n# Make sure mongodb does not get updated automatically\necho 'exclude=mongodb-org*' | sudo tee -a /etc/yum.conf\n\n# Prepare the path for the databases\nsudo chown mongod:mongod ${DB_PATH}\n\n# Enable mongodb to listen for remote connections\nsudo sed -i 's/bindIp:/#bindIp:/g' /etc/mongod.conf\nsudo sed -i \"s#/var/lib/mongo#${DB_PATH}#g\" /etc/mongod.conf\nsudo sed -i 's/27017/37019/g' /etc/mongod.conf\nsudo sed -i 's/#sharding:/sharding:\\n  clusterRole: configsvr/g' /etc/mongod.conf\nsudo sed -i 's/#replication:/replication:\\n  replSetName: configReplSet/g' /etc/mongod.conf\n\n\nconfig_ips=$(echo \"${CONFIG_SRV_IPS}\" | sed 's/^,//' | sed 's/,$//' | tr \",\" \"\\n\")\nconfig_cnt=0\nconfig_str=\"\"\n\nfor config in $config_ips\ndo\n  config_str=$config_str\"{\\\"_id\\\": $config_cnt, host:\\\"$config:37019\\\"},\"\n  config_cnt=$(($config_cnt + 1))\ndone\nconfig_str=`echo $config_str | sed 's/,$//'`\njson=$(cat <<EOF\n{\n  \"_id\": \"configReplSet\",\n  \"members\": [\n    $config_str\n  ]\n}\nEOF\n)\n\nsudo systemctl restart mongod\nsleep 2\n\nsudo mongo --host ${HOST_IP} --port 37019 --eval \"rs.initiate( $json )\"\n\n",
                    "script_type": "sh",
                    "type": "",
                    "command_line_args": "",
                    "login_credential_local_reference": {
                      "kind": "app_credential",
                      "name": "CENTOS",
                      "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                    }
                  },
                  "timeout_secs": "",
                  "type": "EXEC",
                  "variable_list": [],
                  "uuid": "1415310a-d031-4815-a970-bb6e1910fc66"
                }
              ],
              "description": "",
              "name": "fb351d28_runbook",
              "state": "ACTIVE",
              "main_task_local_reference": {
                "kind": "app_task",
                "name": "40b53b04_dag",
                "uuid": "0f7cceaf-0f2b-4fa7-8a44-55483209b3f8"
              },
              "message_list": [],
              "variable_list": [],
              "uuid": "a100d262-774b-4915-962a-4f0f0d56bb0b"
            },
            "type": "",
            "uninstall_runbook": {
              "task_definition_list": [
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVConfigSetPackage",
                    "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                  },
                  "description": "",
                  "message_list": [],
                  "child_tasks_local_reference_list": [
                    {
                      "kind": "app_task",
                      "name": "PackageUninstallTask",
                      "uuid": "b60e1c52-6c40-449f-8558-6b874e274607"
                    }
                  ],
                  "name": "87329188_dag",
                  "state": "ACTIVE",
                  "attrs": {
                    "edges": [],
                    "type": ""
                  },
                  "timeout_secs": "",
                  "type": "DAG",
                  "variable_list": [],
                  "uuid": "92908559-c693-4ed6-b4d4-c9a08046e202"
                },
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVConfigSetPackage",
                    "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
                  },
                  "description": "",
                  "message_list": [],
                  "child_tasks_local_reference_list": [],
                  "name": "PackageUninstallTask",
                  "state": "ACTIVE",
                  "attrs": {
                    "exit_status": [],
                    "script": "#!/bin/bash",
                    "script_type": "sh",
                    "type": "",
                    "command_line_args": "",
                    "login_credential_local_reference": {
                      "kind": "app_credential",
                      "name": "CENTOS",
                      "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                    }
                  },
                  "timeout_secs": "",
                  "type": "EXEC",
                  "variable_list": [],
                  "uuid": "b60e1c52-6c40-449f-8558-6b874e274607"
                }
              ],
              "description": "",
              "name": "d338a852_runbook",
              "state": "ACTIVE",
              "main_task_local_reference": {
                "kind": "app_task",
                "name": "87329188_dag",
                "uuid": "92908559-c693-4ed6-b4d4-c9a08046e202"
              },
              "message_list": [],
              "variable_list": [],
              "uuid": "f0221c91-b30b-45f0-b58c-4e65c1022f93"
            }
          },
          "variable_list": [],
          "uuid": "cf0e6f9c-040b-4757-a7ce-d79df73a00be"
        },
        {
          "description": "",
          "action_list": [
            {
              "description": "System action for installing an application",
              "type": "system",
              "uuid": "7f648731-360f-475b-805a-33f403055e88",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVReplicaSetPackage",
                      "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "PE_Install_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                        "uuid": "cd5c176f-f385-4917-8e91-0d7815d793a2"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "b54010c1-d0fd-4a6c-b329-eb878fa67871"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVReplicaSetPackage",
                      "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "PE_Install_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                    "state": "ACTIVE",
                    "attrs": {
                      "install_runbook": {
                        "task_definition_list": [
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVReplicaSetPackage",
                              "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [
                              {
                                "kind": "app_task",
                                "name": "PackageInstallTask",
                                "uuid": "279eb907-e078-47a0-9e44-946bfe18f7dc"
                              }
                            ],
                            "name": "21686055_dag",
                            "state": "ACTIVE",
                            "attrs": {
                              "edges": [],
                              "type": ""
                            },
                            "timeout_secs": "",
                            "type": "DAG",
                            "variable_list": [],
                            "uuid": "5dbaa935-b4be-4960-9264-f41f47f0b920"
                          },
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVReplicaSetPackage",
                              "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [],
                            "name": "PackageInstallTask",
                            "state": "ACTIVE",
                            "attrs": {
                              "exit_status": [],
                              "script": "#!/bin/bash\nset -ex\nif [[ -f /sys/kernel/mm/transparent_hugepage/enabled ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled\nfi\nif [[ -f /sys/kernel/mm/transparent_hugepage/defrag ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag\nfi\n\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/sysconfig/selinux\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config\nsudo setenforce 0\n\nVERSION=\"3.4.2\"\nDB_PATH=\"@@{DB_PATH}@@\"\nDB_PATH=${DB_PATH:=\"/opt/mongodb\"}\nHOST_IP=\"@@{address}@@\"\narray_index=@@{calm_array_index}@@\nNODES_PER_REPLICA_SET=@@{NODES_PER_REPLICA_SET}@@\nreplica_ips=$(echo @@{calm_array_address}@@ | sed 's/^,//' | sed 's/,$//' | tr \",\" \"\\n\")\n\nsudo mkdir -p ${DB_PATH}\n\necho '[mongodb-org-3.4]\nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/\ngpgcheck=1\nenabled=1\ngpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc' | sudo tee /etc/yum.repos.d/mongodb-3.4.repo\n\n# Add mongodb repository\nsudo yum update -y\n\n# Install mongodb packages\nsudo yum install -y mongodb-org-${VERSION} mongodb-org-server-${VERSION} mongodb-org-shell-${VERSION} mongodb-org-mongos-${VERSION} mongodb-org-tools-${VERSION}\n\n# Make sure mongodb does not get updated automatically\necho 'exclude=mongodb-org*' | sudo tee -a /etc/yum.conf\n\n# Prepare the path for the databases\nsudo chown mongod:mongod ${DB_PATH}\n\n# Enable mongodb to listen for remote connections\ntmp=$((${array_index}+${NODES_PER_REPLICA_SET}))\nreplicasetNo=$((${tmp}/${NODES_PER_REPLICA_SET}))\nreplicaSetName=\"dataReplSet${replicasetNo}\"\n\nsudo sed -i 's/bindIp:/#bindIp:/g' /etc/mongod.conf\nsudo sed -i \"s#/var/lib/mongo#${DB_PATH}#g\" /etc/mongod.conf\nsudo sed -i \"s/#replication:/replication:\\n  replSetName: $replicaSetName/g\" /etc/mongod.conf\nsudo sed -i 's/#sharding:/sharding:\\n  clusterRole: shardsvr/g' /etc/mongod.conf\n\n### Getting replicaset members details \nreplica_cnt=0\ndeclare -a replicaset_members\n\nfor replica in $replica_ips\ndo\n  tmp=$((${replica_cnt}+${NODES_PER_REPLICA_SET}))\n  i=\"$((${tmp}/${NODES_PER_REPLICA_SET}))\"\n  replicaset_members[$i]=${replicaset_members[$i]}\"{\\\"_id\\\": $replica_cnt, host:\\\"$replica:27017\\\"},\"\n  replica_cnt=$(($replica_cnt + 1))\ndone\n\nsudo systemctl restart mongod\nsleep 2\n\n### Initiating replicaset \njson=$(cat <<EOF\n{\n  \"_id\": \"$replicaSetName\",\n  \"members\": [ ${replicaset_members[$replicasetNo]}]\n}\nEOF\n)\nsudo mongo --host ${HOST_IP} --port 27017 --eval \"rs.initiate( $json )\"\nsleep 2\n",
                              "script_type": "sh",
                              "type": "",
                              "command_line_args": "",
                              "login_credential_local_reference": {
                                "kind": "app_credential",
                                "name": "CENTOS",
                                "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                              }
                            },
                            "timeout_secs": "",
                            "type": "EXEC",
                            "variable_list": [],
                            "uuid": "279eb907-e078-47a0-9e44-946bfe18f7dc"
                          }
                        ],
                        "description": "",
                        "name": "0aecfbfc_runbook",
                        "state": "ACTIVE",
                        "main_task_local_reference": {
                          "kind": "app_task",
                          "name": "21686055_dag",
                          "uuid": "5dbaa935-b4be-4960-9264-f41f47f0b920"
                        },
                        "message_list": [],
                        "variable_list": [],
                        "uuid": "a87cda80-da4a-4984-8283-4670aab78768"
                      },
                      "type": "",
                      "uninstall_runbook": {
                        "task_definition_list": [
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVReplicaSetPackage",
                              "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [
                              {
                                "kind": "app_task",
                                "name": "PackageUninstallTask",
                                "uuid": "a4735296-71fa-41a0-87cc-ae83cc4d7e76"
                              }
                            ],
                            "name": "46111458_dag",
                            "state": "ACTIVE",
                            "attrs": {
                              "edges": [],
                              "type": ""
                            },
                            "timeout_secs": "",
                            "type": "DAG",
                            "variable_list": [],
                            "uuid": "5dfea754-d453-4806-9a98-198a106fcd9e"
                          },
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVReplicaSetPackage",
                              "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [],
                            "name": "PackageUninstallTask",
                            "state": "ACTIVE",
                            "attrs": {
                              "exit_status": [],
                              "script": "#!/bin/bash",
                              "script_type": "sh",
                              "type": "",
                              "command_line_args": "",
                              "login_credential_local_reference": {
                                "kind": "app_credential",
                                "name": "CENTOS",
                                "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                              }
                            },
                            "timeout_secs": "",
                            "type": "EXEC",
                            "variable_list": [],
                            "uuid": "a4735296-71fa-41a0-87cc-ae83cc4d7e76"
                          }
                        ],
                        "description": "",
                        "name": "d38a2e0d_runbook",
                        "state": "ACTIVE",
                        "main_task_local_reference": {
                          "kind": "app_task",
                          "name": "46111458_dag",
                          "uuid": "5dfea754-d453-4806-9a98-198a106fcd9e"
                        },
                        "message_list": [],
                        "variable_list": [],
                        "uuid": "de7de06a-085d-4863-93be-0f1f9c9f2438"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CUSTOM_PACKAGE_INSTALL",
                    "variable_list": [],
                    "uuid": "cd5c176f-f385-4917-8e91-0d7815d793a2"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                  "uuid": "b54010c1-d0fd-4a6c-b329-eb878fa67871"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "015de7d5-5fe6-4d2a-82cc-2e08ad07cb68"
              },
              "message_list": [],
              "name": "action_install"
            },
            {
              "description": "System action for creating an application",
              "type": "system",
              "uuid": "80e91696-d7e8-4156-a0f0-82311ff994b7",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVReplicaSetPackage",
                      "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__install_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                        "uuid": "7a3f1057-47c6-4199-9468-17d0d61935e5"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__create_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "45f5c742-c38c-4dba-b52e-c2c17b1f24ab"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "e058965a-47fa-4110-bbab-9c3a33c4925d"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "45f5c742-c38c-4dba-b52e-c2c17b1f24ab"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "64df9677-7793-4a24-9272-3d893ffc094c",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "e058965a-47fa-4110-bbab-9c3a33c4925d"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__install_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                            "uuid": "7a3f1057-47c6-4199-9468-17d0d61935e5"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "81b3e681-6314-4a6b-a617-feeab85baa77",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__create_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "45f5c742-c38c-4dba-b52e-c2c17b1f24ab"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "39c34ab6-bd2e-4f58-bfdd-927844568132"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVReplicaSetPackage",
                      "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__install_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                        "uuid": "015de7d5-5fe6-4d2a-82cc-2e08ad07cb68"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "7a3f1057-47c6-4199-9468-17d0d61935e5"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__create_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "e984fac1_runbook",
                        "uuid": "c4094263-6a43-46f6-9b5e-335b65860978"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "45f5c742-c38c-4dba-b52e-c2c17b1f24ab"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "6286b364_runbook",
                        "uuid": "b414ddcc-2221-4078-bac1-8c739a480a49"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "e058965a-47fa-4110-bbab-9c3a33c4925d"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                  "uuid": "39c34ab6-bd2e-4f58-bfdd-927844568132"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "9581a909-b8b9-4c2f-94fb-993c06aa4a21"
              },
              "message_list": [],
              "name": "action_create"
            },
            {
              "description": "System action for uninstalling an application",
              "type": "system",
              "uuid": "13d71fac-fb2c-4db3-823a-00a812fa0328",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVReplicaSetPackage",
                      "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "PE_Uninstall_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                        "uuid": "a4ff7ee9-55b7-4681-8b26-5bb169542d22"
                      }
                    ],
                    "name": "SYS_GEN__DAG_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "0b2f3ef5-0f5f-459f-9bd7-9c076b5a59a8"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVReplicaSetPackage",
                      "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "PE_Uninstall_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                    "state": "ACTIVE",
                    "attrs": {
                      "install_runbook": {
                        "task_definition_list": [
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVReplicaSetPackage",
                              "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [
                              {
                                "kind": "app_task",
                                "name": "PackageInstallTask",
                                "uuid": "279eb907-e078-47a0-9e44-946bfe18f7dc"
                              }
                            ],
                            "name": "21686055_dag",
                            "state": "ACTIVE",
                            "attrs": {
                              "edges": [],
                              "type": ""
                            },
                            "timeout_secs": "",
                            "type": "DAG",
                            "variable_list": [],
                            "uuid": "5dbaa935-b4be-4960-9264-f41f47f0b920"
                          },
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVReplicaSetPackage",
                              "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [],
                            "name": "PackageInstallTask",
                            "state": "ACTIVE",
                            "attrs": {
                              "exit_status": [],
                              "script": "#!/bin/bash\nset -ex\nif [[ -f /sys/kernel/mm/transparent_hugepage/enabled ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled\nfi\nif [[ -f /sys/kernel/mm/transparent_hugepage/defrag ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag\nfi\n\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/sysconfig/selinux\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config\nsudo setenforce 0\n\nVERSION=\"3.4.2\"\nDB_PATH=\"@@{DB_PATH}@@\"\nDB_PATH=${DB_PATH:=\"/opt/mongodb\"}\nHOST_IP=\"@@{address}@@\"\narray_index=@@{calm_array_index}@@\nNODES_PER_REPLICA_SET=@@{NODES_PER_REPLICA_SET}@@\nreplica_ips=$(echo @@{calm_array_address}@@ | sed 's/^,//' | sed 's/,$//' | tr \",\" \"\\n\")\n\nsudo mkdir -p ${DB_PATH}\n\necho '[mongodb-org-3.4]\nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/\ngpgcheck=1\nenabled=1\ngpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc' | sudo tee /etc/yum.repos.d/mongodb-3.4.repo\n\n# Add mongodb repository\nsudo yum update -y\n\n# Install mongodb packages\nsudo yum install -y mongodb-org-${VERSION} mongodb-org-server-${VERSION} mongodb-org-shell-${VERSION} mongodb-org-mongos-${VERSION} mongodb-org-tools-${VERSION}\n\n# Make sure mongodb does not get updated automatically\necho 'exclude=mongodb-org*' | sudo tee -a /etc/yum.conf\n\n# Prepare the path for the databases\nsudo chown mongod:mongod ${DB_PATH}\n\n# Enable mongodb to listen for remote connections\ntmp=$((${array_index}+${NODES_PER_REPLICA_SET}))\nreplicasetNo=$((${tmp}/${NODES_PER_REPLICA_SET}))\nreplicaSetName=\"dataReplSet${replicasetNo}\"\n\nsudo sed -i 's/bindIp:/#bindIp:/g' /etc/mongod.conf\nsudo sed -i \"s#/var/lib/mongo#${DB_PATH}#g\" /etc/mongod.conf\nsudo sed -i \"s/#replication:/replication:\\n  replSetName: $replicaSetName/g\" /etc/mongod.conf\nsudo sed -i 's/#sharding:/sharding:\\n  clusterRole: shardsvr/g' /etc/mongod.conf\n\n### Getting replicaset members details \nreplica_cnt=0\ndeclare -a replicaset_members\n\nfor replica in $replica_ips\ndo\n  tmp=$((${replica_cnt}+${NODES_PER_REPLICA_SET}))\n  i=\"$((${tmp}/${NODES_PER_REPLICA_SET}))\"\n  replicaset_members[$i]=${replicaset_members[$i]}\"{\\\"_id\\\": $replica_cnt, host:\\\"$replica:27017\\\"},\"\n  replica_cnt=$(($replica_cnt + 1))\ndone\n\nsudo systemctl restart mongod\nsleep 2\n\n### Initiating replicaset \njson=$(cat <<EOF\n{\n  \"_id\": \"$replicaSetName\",\n  \"members\": [ ${replicaset_members[$replicasetNo]}]\n}\nEOF\n)\nsudo mongo --host ${HOST_IP} --port 27017 --eval \"rs.initiate( $json )\"\nsleep 2\n",
                              "script_type": "sh",
                              "type": "",
                              "command_line_args": "",
                              "login_credential_local_reference": {
                                "kind": "app_credential",
                                "name": "CENTOS",
                                "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                              }
                            },
                            "timeout_secs": "",
                            "type": "EXEC",
                            "variable_list": [],
                            "uuid": "279eb907-e078-47a0-9e44-946bfe18f7dc"
                          }
                        ],
                        "description": "",
                        "name": "0aecfbfc_runbook",
                        "state": "ACTIVE",
                        "main_task_local_reference": {
                          "kind": "app_task",
                          "name": "21686055_dag",
                          "uuid": "5dbaa935-b4be-4960-9264-f41f47f0b920"
                        },
                        "message_list": [],
                        "variable_list": [],
                        "uuid": "a87cda80-da4a-4984-8283-4670aab78768"
                      },
                      "type": "",
                      "uninstall_runbook": {
                        "task_definition_list": [
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVReplicaSetPackage",
                              "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [
                              {
                                "kind": "app_task",
                                "name": "PackageUninstallTask",
                                "uuid": "a4735296-71fa-41a0-87cc-ae83cc4d7e76"
                              }
                            ],
                            "name": "46111458_dag",
                            "state": "ACTIVE",
                            "attrs": {
                              "edges": [],
                              "type": ""
                            },
                            "timeout_secs": "",
                            "type": "DAG",
                            "variable_list": [],
                            "uuid": "5dfea754-d453-4806-9a98-198a106fcd9e"
                          },
                          {
                            "target_any_local_reference": {
                              "kind": "app_package",
                              "name": "AHVReplicaSetPackage",
                              "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                            },
                            "description": "",
                            "message_list": [],
                            "child_tasks_local_reference_list": [],
                            "name": "PackageUninstallTask",
                            "state": "ACTIVE",
                            "attrs": {
                              "exit_status": [],
                              "script": "#!/bin/bash",
                              "script_type": "sh",
                              "type": "",
                              "command_line_args": "",
                              "login_credential_local_reference": {
                                "kind": "app_credential",
                                "name": "CENTOS",
                                "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                              }
                            },
                            "timeout_secs": "",
                            "type": "EXEC",
                            "variable_list": [],
                            "uuid": "a4735296-71fa-41a0-87cc-ae83cc4d7e76"
                          }
                        ],
                        "description": "",
                        "name": "d38a2e0d_runbook",
                        "state": "ACTIVE",
                        "main_task_local_reference": {
                          "kind": "app_task",
                          "name": "46111458_dag",
                          "uuid": "5dfea754-d453-4806-9a98-198a106fcd9e"
                        },
                        "message_list": [],
                        "variable_list": [],
                        "uuid": "de7de06a-085d-4863-93be-0f1f9c9f2438"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CUSTOM_PACKAGE_UNINSTALL",
                    "variable_list": [],
                    "uuid": "a4ff7ee9-55b7-4681-8b26-5bb169542d22"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__DAG_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                  "uuid": "0b2f3ef5-0f5f-459f-9bd7-9c076b5a59a8"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "7a4f2ebd-ee07-4fec-973d-0a284ae72425"
              },
              "message_list": [],
              "name": "action_uninstall"
            },
            {
              "description": "System action for deleting an application. Deletes physical machines as well",
              "type": "system",
              "uuid": "d0b8e462-f2d5-41e7-b0c5-64c55d070ddc",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVReplicaSetPackage",
                      "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "572f8607-a10c-4880-94a8-81899b795aa5"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "6a396da0-6998-4d99-b58f-10e5602c35ef"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__uninstall_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                        "uuid": "5aab7eaf-faeb-496c-80da-04ec03147ac0"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "572f8607-a10c-4880-94a8-81899b795aa5"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "b69b1444-c5ff-4be8-9678-686cf780ebdd",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "6a396da0-6998-4d99-b58f-10e5602c35ef"
                          }
                        },
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "6a396da0-6998-4d99-b58f-10e5602c35ef"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "a4090278-98ea-47b0-a0dc-435306cd8952",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__uninstall_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                            "uuid": "5aab7eaf-faeb-496c-80da-04ec03147ac0"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "c952e620-d863-4597-9933-c1f0838b47d9"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "ee3949e3_runbook",
                        "uuid": "706d1aab-530f-452d-8b33-15bfbdb2fdd7"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "572f8607-a10c-4880-94a8-81899b795aa5"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "96df4d94_runbook",
                        "uuid": "8d242223-afda-4c13-b35f-b9dff6717e5e"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "6a396da0-6998-4d99-b58f-10e5602c35ef"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVReplicaSetPackage",
                      "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__uninstall_CRb_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                        "uuid": "7a4f2ebd-ee07-4fec-973d-0a284ae72425"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "5aab7eaf-faeb-496c-80da-04ec03147ac0"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                  "uuid": "c952e620-d863-4597-9933-c1f0838b47d9"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "416f7f80-590c-4287-8911-4f207577d6cd"
              },
              "message_list": [],
              "name": "action_delete"
            },
            {
              "description": "System action for deleting an application. Does not delete physical machines",
              "type": "system",
              "uuid": "c85a6592-cf23-45a6-bd4a-54e5c857118f",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVReplicaSetPackage",
                      "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__soft_delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "43961e8e-2cd0-4603-ab3a-47aa59d79b28"
                      },
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__Package_Element_Delete_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                        "uuid": "63e1b285-8ba9-459b-9522-7947ae6d64b7"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [
                        {
                          "from_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__soft_delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                            "uuid": "43961e8e-2cd0-4603-ab3a-47aa59d79b28"
                          },
                          "edge_type": "inherent",
                          "type": "",
                          "uuid": "2ce88482-d727-42d7-8e68-ec484de8fb97",
                          "to_task_reference": {
                            "kind": "app_task",
                            "name": "SYS_GEN__Package_Element_Delete_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                            "uuid": "63e1b285-8ba9-459b-9522-7947ae6d64b7"
                          }
                        }
                      ],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "686f7801-e1c1-410c-9866-6bf48e9091be"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__soft_delete_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "SYS_GEN__Runbook_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "c0f5e1e4-bdb8-4981-baa5-7f01c4a753ad"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "43961e8e-2cd0-4603-ab3a-47aa59d79b28"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVReplicaSetPackage",
                      "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__Package_Element_Delete_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "SOFT_DELETE_ELEMENT",
                    "variable_list": [],
                    "uuid": "63e1b285-8ba9-459b-9522-7947ae6d64b7"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                  "uuid": "686f7801-e1c1-410c-9866-6bf48e9091be"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "21550a57-8113-4b8d-9f00-e08333d59bef"
              },
              "message_list": [],
              "name": "action_soft_delete"
            },
            {
              "description": "System action for starting an application",
              "type": "system",
              "uuid": "026ad7d9-7b5d-494a-a18e-24f1c3c936c2",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVReplicaSetPackage",
                      "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "ae75edb8-be3b-44ec-b8c6-1bc3b72798cd"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "d8c65b3c-995e-4b32-b865-252e538d3fe6"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__start_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "6286b364_runbook",
                        "uuid": "b414ddcc-2221-4078-bac1-8c739a480a49"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "ae75edb8-be3b-44ec-b8c6-1bc3b72798cd"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                  "uuid": "d8c65b3c-995e-4b32-b865-252e538d3fe6"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "a50a5e4b-f65d-4ab9-9d4c-6d9b03d95de2"
              },
              "message_list": [],
              "name": "action_start"
            },
            {
              "description": "System action for stopping an application",
              "type": "system",
              "uuid": "475962d9-f5a5-4598-9f55-1aeb341d1175",
              "state": "ACTIVE",
              "critical": true,
              "attrs": {},
              "runbook": {
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_package",
                      "name": "AHVReplicaSetPackage",
                      "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                        "uuid": "3aef674d-f1e6-4ac6-a4c6-a4dc3bc8017e"
                      }
                    ],
                    "name": "SYS_GEN__Composite_DAG_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                    "state": "ACTIVE",
                    "attrs": {
                      "edges": [],
                      "type": "DAG"
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "uuid": "803d7446-dc68-4968-aae8-dd52cb45cd67"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet",
                      "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
                    },
                    "description": "",
                    "message_list": [],
                    "child_tasks_local_reference_list": [],
                    "name": "SYS_GEN__stop_CRb_Service_c8ca5f66_518f_42ca_8b76_0b8bfaca4bc7",
                    "state": "ACTIVE",
                    "attrs": {
                      "type": "CALL_RUNBOOK",
                      "inarg_list": [],
                      "runbook_reference": {
                        "kind": "app_runbook",
                        "name": "ee3949e3_runbook",
                        "uuid": "706d1aab-530f-452d-8b33-15bfbdb2fdd7"
                      }
                    },
                    "timeout_secs": "",
                    "type": "CALL_RUNBOOK",
                    "variable_list": [],
                    "uuid": "3aef674d-f1e6-4ac6-a4c6-a4dc3bc8017e"
                  }
                ],
                "description": "",
                "name": "SYS_GEN__Runbook_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                "state": "ACTIVE",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "SYS_GEN__Composite_DAG_Package_c8edc882_7aac_49b4_9656_2bf2c26701b0",
                  "uuid": "803d7446-dc68-4968-aae8-dd52cb45cd67"
                },
                "message_list": [],
                "variable_list": [],
                "uuid": "aa646637-78b2-4fa5-89d0-3f4a752d3584"
              },
              "message_list": [],
              "name": "action_stop"
            }
          ],
          "type": "DEB",
          "service_local_reference_list": [
            {
              "kind": "app_service",
              "name": "Mongo_ReplicaSet",
              "uuid": "c8ca5f66-518f-42ca-8b76-0b8bfaca4bc7"
            }
          ],
          "name": "AHVReplicaSetPackage",
          "state": "ACTIVE",
          "version": "",
          "editables": {},
          "message_list": [],
          "options": {
            "install_runbook": {
              "task_definition_list": [
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVReplicaSetPackage",
                    "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                  },
                  "description": "",
                  "message_list": [],
                  "child_tasks_local_reference_list": [
                    {
                      "kind": "app_task",
                      "name": "PackageInstallTask",
                      "uuid": "279eb907-e078-47a0-9e44-946bfe18f7dc"
                    }
                  ],
                  "name": "21686055_dag",
                  "state": "ACTIVE",
                  "attrs": {
                    "edges": [],
                    "type": ""
                  },
                  "timeout_secs": "",
                  "type": "DAG",
                  "variable_list": [],
                  "uuid": "5dbaa935-b4be-4960-9264-f41f47f0b920"
                },
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVReplicaSetPackage",
                    "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                  },
                  "description": "",
                  "message_list": [],
                  "child_tasks_local_reference_list": [],
                  "name": "PackageInstallTask",
                  "state": "ACTIVE",
                  "attrs": {
                    "exit_status": [],
                    "script": "#!/bin/bash\nset -ex\nif [[ -f /sys/kernel/mm/transparent_hugepage/enabled ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled\nfi\nif [[ -f /sys/kernel/mm/transparent_hugepage/defrag ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag\nfi\n\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/sysconfig/selinux\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config\nsudo setenforce 0\n\nVERSION=\"3.4.2\"\nDB_PATH=\"@@{DB_PATH}@@\"\nDB_PATH=${DB_PATH:=\"/opt/mongodb\"}\nHOST_IP=\"@@{address}@@\"\narray_index=@@{calm_array_index}@@\nNODES_PER_REPLICA_SET=@@{NODES_PER_REPLICA_SET}@@\nreplica_ips=$(echo @@{calm_array_address}@@ | sed 's/^,//' | sed 's/,$//' | tr \",\" \"\\n\")\n\nsudo mkdir -p ${DB_PATH}\n\necho '[mongodb-org-3.4]\nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/\ngpgcheck=1\nenabled=1\ngpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc' | sudo tee /etc/yum.repos.d/mongodb-3.4.repo\n\n# Add mongodb repository\nsudo yum update -y\n\n# Install mongodb packages\nsudo yum install -y mongodb-org-${VERSION} mongodb-org-server-${VERSION} mongodb-org-shell-${VERSION} mongodb-org-mongos-${VERSION} mongodb-org-tools-${VERSION}\n\n# Make sure mongodb does not get updated automatically\necho 'exclude=mongodb-org*' | sudo tee -a /etc/yum.conf\n\n# Prepare the path for the databases\nsudo chown mongod:mongod ${DB_PATH}\n\n# Enable mongodb to listen for remote connections\ntmp=$((${array_index}+${NODES_PER_REPLICA_SET}))\nreplicasetNo=$((${tmp}/${NODES_PER_REPLICA_SET}))\nreplicaSetName=\"dataReplSet${replicasetNo}\"\n\nsudo sed -i 's/bindIp:/#bindIp:/g' /etc/mongod.conf\nsudo sed -i \"s#/var/lib/mongo#${DB_PATH}#g\" /etc/mongod.conf\nsudo sed -i \"s/#replication:/replication:\\n  replSetName: $replicaSetName/g\" /etc/mongod.conf\nsudo sed -i 's/#sharding:/sharding:\\n  clusterRole: shardsvr/g' /etc/mongod.conf\n\n### Getting replicaset members details \nreplica_cnt=0\ndeclare -a replicaset_members\n\nfor replica in $replica_ips\ndo\n  tmp=$((${replica_cnt}+${NODES_PER_REPLICA_SET}))\n  i=\"$((${tmp}/${NODES_PER_REPLICA_SET}))\"\n  replicaset_members[$i]=${replicaset_members[$i]}\"{\\\"_id\\\": $replica_cnt, host:\\\"$replica:27017\\\"},\"\n  replica_cnt=$(($replica_cnt + 1))\ndone\n\nsudo systemctl restart mongod\nsleep 2\n\n### Initiating replicaset \njson=$(cat <<EOF\n{\n  \"_id\": \"$replicaSetName\",\n  \"members\": [ ${replicaset_members[$replicasetNo]}]\n}\nEOF\n)\nsudo mongo --host ${HOST_IP} --port 27017 --eval \"rs.initiate( $json )\"\nsleep 2\n",
                    "script_type": "sh",
                    "type": "",
                    "command_line_args": "",
                    "login_credential_local_reference": {
                      "kind": "app_credential",
                      "name": "CENTOS",
                      "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                    }
                  },
                  "timeout_secs": "",
                  "type": "EXEC",
                  "variable_list": [],
                  "uuid": "279eb907-e078-47a0-9e44-946bfe18f7dc"
                }
              ],
              "description": "",
              "name": "0aecfbfc_runbook",
              "state": "ACTIVE",
              "main_task_local_reference": {
                "kind": "app_task",
                "name": "21686055_dag",
                "uuid": "5dbaa935-b4be-4960-9264-f41f47f0b920"
              },
              "message_list": [],
              "variable_list": [],
              "uuid": "a87cda80-da4a-4984-8283-4670aab78768"
            },
            "type": "",
            "uninstall_runbook": {
              "task_definition_list": [
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVReplicaSetPackage",
                    "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                  },
                  "description": "",
                  "message_list": [],
                  "child_tasks_local_reference_list": [
                    {
                      "kind": "app_task",
                      "name": "PackageUninstallTask",
                      "uuid": "a4735296-71fa-41a0-87cc-ae83cc4d7e76"
                    }
                  ],
                  "name": "46111458_dag",
                  "state": "ACTIVE",
                  "attrs": {
                    "edges": [],
                    "type": ""
                  },
                  "timeout_secs": "",
                  "type": "DAG",
                  "variable_list": [],
                  "uuid": "5dfea754-d453-4806-9a98-198a106fcd9e"
                },
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVReplicaSetPackage",
                    "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
                  },
                  "description": "",
                  "message_list": [],
                  "child_tasks_local_reference_list": [],
                  "name": "PackageUninstallTask",
                  "state": "ACTIVE",
                  "attrs": {
                    "exit_status": [],
                    "script": "#!/bin/bash",
                    "script_type": "sh",
                    "type": "",
                    "command_line_args": "",
                    "login_credential_local_reference": {
                      "kind": "app_credential",
                      "name": "CENTOS",
                      "uuid": "f95c5661-fac8-4c1b-b74c-1fab525c343d"
                    }
                  },
                  "timeout_secs": "",
                  "type": "EXEC",
                  "variable_list": [],
                  "uuid": "a4735296-71fa-41a0-87cc-ae83cc4d7e76"
                }
              ],
              "description": "",
              "name": "d38a2e0d_runbook",
              "state": "ACTIVE",
              "main_task_local_reference": {
                "kind": "app_task",
                "name": "46111458_dag",
                "uuid": "5dfea754-d453-4806-9a98-198a106fcd9e"
              },
              "message_list": [],
              "variable_list": [],
              "uuid": "de7de06a-085d-4863-93be-0f1f9c9f2438"
            }
          },
          "variable_list": [],
          "uuid": "c8edc882-7aac-49b4-9656-2bf2c26701b0"
        },
        {
          "description": "",
          "action_list": [],
          "type": "SUBSTRATE_IMAGE",
          "service_local_reference_list": [],
          "name": "CENTOS_ConfigSet",
          "state": "ACTIVE",
          "version": "",
          "editables": {},
          "message_list": [],
          "options": {
            "type": "",
            "name": "CentOs-7.4-Mongo-ConfigSet",
            "resources": {
              "image_type": "DISK_IMAGE",
              "checksum": {
                "checksum_algorithm": "",
                "type": "",
                "checksum_value": ""
              },
              "source_uri": "http://download.nutanix.com/calm/CentOS-7-x86_64-GenericCloud.qcow2",
              "version": {
                "product_version": "",
                "type": "",
                "product_name": ""
              },
              "architecture": "X86_64",
              "type": ""
            },
            "description": ""
          },
          "variable_list": [],
          "uuid": "ccf275fd-1e2b-47b0-9a19-4c7ca7999c09"
        },
        {
          "description": "",
          "action_list": [],
          "type": "SUBSTRATE_IMAGE",
          "service_local_reference_list": [],
          "name": "CENTOS_ReplicaSet",
          "state": "ACTIVE",
          "version": "",
          "editables": {},
          "message_list": [],
          "options": {
            "type": "",
            "name": "CentOs-7.4-Mongo-ReplicaSet",
            "resources": {
              "image_type": "DISK_IMAGE",
              "checksum": {
                "checksum_algorithm": "",
                "type": "",
                "checksum_value": ""
              },
              "source_uri": "http://download.nutanix.com/calm/CentOS-7-x86_64-GenericCloud.qcow2",
              "version": {
                "product_version": "",
                "type": "",
                "product_name": ""
              },
              "architecture": "X86_64",
              "type": ""
            },
            "description": ""
          },
          "variable_list": [],
          "uuid": "0975e86e-8135-4dab-a091-c25b8fef336a"
        },
        {
          "description": "",
          "action_list": [],
          "type": "SUBSTRATE_IMAGE",
          "service_local_reference_list": [],
          "name": "CENTOS_Router",
          "state": "ACTIVE",
          "version": "",
          "editables": {},
          "message_list": [],
          "options": {
            "type": "",
            "name": "CentOs-7.4-Mongo-Router",
            "resources": {
              "image_type": "DISK_IMAGE",
              "checksum": {
                "checksum_algorithm": "",
                "type": "",
                "checksum_value": ""
              },
              "source_uri": "http://download.nutanix.com/calm/CentOS-7-x86_64-GenericCloud.qcow2",
              "version": {
                "product_version": "",
                "type": "",
                "product_name": ""
              },
              "architecture": "X86_64",
              "type": ""
            },
            "description": ""
          },
          "variable_list": [],
          "uuid": "2190e33f-65cb-410d-9121-a7b188aa0a99"
        }
      ]
    },
    "name": "Import_API_Lab"
  },
  "spec": {
    "description": "Accessibility:\n* Using mongodb command line `sudo mongo --host [IP_ADDRESS] --port 27017`\n",
    "resources": {
      "service_definition_list": [
        {
          "singleton": false,
          "name": "Mongo_Router",
          "action_list": [
            {
              "critical": false,
              "type": "system",
              "description": "System action for creating an application",
              "name": "action_create",
              "runbook": {
                "variable_list": [],
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [],
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "name": "afc1e408_dag"
                  }
                ],
                "description": "",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "afc1e408_dag"
                },
                "name": "b9357b85_runbook"
              }
            },
            {
              "critical": false,
              "type": "system",
              "description": "System action for deleting an application. Deletes physical machines as well",
              "name": "action_delete",
              "runbook": {
                "variable_list": [],
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [],
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "name": "b0b70621_dag"
                  }
                ],
                "description": "",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "b0b70621_dag"
                },
                "name": "10af4ae1_runbook"
              }
            },
            {
              "critical": false,
              "type": "system",
              "description": "System action for starting an application",
              "name": "action_start",
              "runbook": {
                "variable_list": [],
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [
                      {
                        "kind": "app_task",
                        "name": "ConfigureShards"
                      }
                    ],
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "name": "7f27b711_dag"
                  },
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [],
                    "attrs": {
                      "exit_status": [],
                      "script": "#!/bin/bash\nNODES_PER_REPLICA_SET=\"@@{NODES_PER_REPLICA_SET}@@\"\nHOST_IP=\"@@{address}@@\"\nreplica_ips=$(echo @@{Mongo_ReplicaSet.address}@@ | sed 's/^,//' | sed 's/,$//' | tr \",\" \"\\n\")\n\n\n### Setting up shards\nreplica_cnt=0\nfor replica in $replica_ips\ndo\n  tmp=$((${replica_cnt}+${NODES_PER_REPLICA_SET}))\n  replicaSetName=\"dataReplSet$((${tmp}/${NODES_PER_REPLICA_SET}))\"\n  sudo mongo --host ${HOST_IP} --port 27017 --eval \"sh.addShard( \\\"$replicaSetName/$replica:27017\\\" )\"\n  replica_cnt=$(($replica_cnt + 1))\ndone\n\n",
                      "script_type": "sh",
                      "type": "",
                      "command_line_args": "",
                      "login_credential_local_reference": {
                        "kind": "app_credential",
                        "name": "CENTOS"
                      }
                    },
                    "timeout_secs": "",
                    "type": "EXEC",
                    "variable_list": [],
                    "name": "ConfigureShards"
                  }
                ],
                "description": "",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "7f27b711_dag"
                },
                "name": "d9638df2_runbook"
              }
            },
            {
              "critical": false,
              "type": "system",
              "description": "System action for stopping an application",
              "name": "action_stop",
              "runbook": {
                "variable_list": [],
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [],
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "name": "418ea855_dag"
                  }
                ],
                "description": "",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "418ea855_dag"
                },
                "name": "507a01b5_runbook"
              }
            },
            {
              "critical": false,
              "type": "system",
              "description": "System action for restarting an application",
              "name": "action_restart",
              "runbook": {
                "variable_list": [],
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_Router"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [],
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "name": "b5c5a93d_dag"
                  }
                ],
                "description": "",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "b5c5a93d_dag"
                },
                "name": "c46e4637_runbook"
              }
            }
          ],
          "description": "",
          "port_list": [],
          "tier": "",
          "variable_list": [],
          "depends_on_list": []
        },
        {
          "singleton": false,
          "name": "Mongo_ConfigSet",
          "action_list": [
            {
              "critical": false,
              "type": "system",
              "description": "System action for creating an application",
              "name": "action_create",
              "runbook": {
                "variable_list": [],
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [],
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "name": "8c950d87_dag"
                  }
                ],
                "description": "",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "8c950d87_dag"
                },
                "name": "e8608054_runbook"
              }
            },
            {
              "critical": false,
              "type": "system",
              "description": "System action for deleting an application. Deletes physical machines as well",
              "name": "action_delete",
              "runbook": {
                "variable_list": [],
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [],
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "name": "3eb93418_dag"
                  }
                ],
                "description": "",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "3eb93418_dag"
                },
                "name": "fd485dc4_runbook"
              }
            },
            {
              "critical": false,
              "type": "system",
              "description": "System action for starting an application",
              "name": "action_start",
              "runbook": {
                "variable_list": [],
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [],
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "name": "8488e56f_dag"
                  }
                ],
                "description": "",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "8488e56f_dag"
                },
                "name": "d04295dc_runbook"
              }
            },
            {
              "critical": false,
              "type": "system",
              "description": "System action for stopping an application",
              "name": "action_stop",
              "runbook": {
                "variable_list": [],
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [],
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "name": "1000364d_dag"
                  }
                ],
                "description": "",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "1000364d_dag"
                },
                "name": "c93943b8_runbook"
              }
            },
            {
              "critical": false,
              "type": "system",
              "description": "System action for restarting an application",
              "name": "action_restart",
              "runbook": {
                "variable_list": [],
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ConfigSet"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [],
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "name": "646ec79e_dag"
                  }
                ],
                "description": "",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "646ec79e_dag"
                },
                "name": "529ff634_runbook"
              }
            }
          ],
          "description": "",
          "port_list": [],
          "tier": "",
          "variable_list": [],
          "depends_on_list": []
        },
        {
          "singleton": false,
          "name": "Mongo_ReplicaSet",
          "action_list": [
            {
              "critical": false,
              "type": "system",
              "description": "System action for creating an application",
              "name": "action_create",
              "runbook": {
                "variable_list": [],
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [],
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "name": "3f585e04_dag"
                  }
                ],
                "description": "",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "3f585e04_dag"
                },
                "name": "e984fac1_runbook"
              }
            },
            {
              "critical": false,
              "type": "system",
              "description": "System action for deleting an application. Deletes physical machines as well",
              "name": "action_delete",
              "runbook": {
                "variable_list": [],
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [],
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "name": "2e34dabb_dag"
                  }
                ],
                "description": "",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "2e34dabb_dag"
                },
                "name": "96df4d94_runbook"
              }
            },
            {
              "critical": false,
              "type": "system",
              "description": "System action for starting an application",
              "name": "action_start",
              "runbook": {
                "variable_list": [],
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [],
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "name": "e00c41e9_dag"
                  }
                ],
                "description": "",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "e00c41e9_dag"
                },
                "name": "6286b364_runbook"
              }
            },
            {
              "critical": false,
              "type": "system",
              "description": "System action for stopping an application",
              "name": "action_stop",
              "runbook": {
                "variable_list": [],
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [],
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "name": "01c64f18_dag"
                  }
                ],
                "description": "",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "01c64f18_dag"
                },
                "name": "ee3949e3_runbook"
              }
            },
            {
              "critical": false,
              "type": "system",
              "description": "System action for restarting an application",
              "name": "action_restart",
              "runbook": {
                "variable_list": [],
                "task_definition_list": [
                  {
                    "target_any_local_reference": {
                      "kind": "app_service",
                      "name": "Mongo_ReplicaSet"
                    },
                    "description": "",
                    "child_tasks_local_reference_list": [],
                    "attrs": {
                      "edges": [],
                      "type": ""
                    },
                    "timeout_secs": "",
                    "type": "DAG",
                    "variable_list": [],
                    "name": "52c456b0_dag"
                  }
                ],
                "description": "",
                "main_task_local_reference": {
                  "kind": "app_task",
                  "name": "52c456b0_dag"
                },
                "name": "0ebe0bd0_runbook"
              }
            }
          ],
          "description": "",
          "port_list": [],
          "tier": "",
          "variable_list": [],
          "depends_on_list": []
        }
      ],
      "substrate_definition_list": [
        {
          "description": "",
          "action_list": [],
          "readiness_probe": {
            "connection_type": "SSH",
            "disable_readiness_probe": false,
            "timeout_secs": "60",
            "address": "@@{platform.status.resources.nic_list[0].ip_endpoint_list[0].ip}@@",
            "connection_port": 22,
            "login_credential_local_reference": {
              "kind": "app_credential",
              "name": "CENTOS"
            }
          },
          "editables": {
            "readiness_probe": {
              "connection_port": true,
              "timeout_secs": true
            },
            "create_spec": {
              "name": true,
              "resources": {
                "nic_list": {
                  "0": {
                    "subnet_reference": true
                  }
                },
                "num_vcpus_per_socket": true,
                "num_sockets": true,
                "memory_size_mib": true,
                "guest_customization": true,
                "disk_list": {
                  "0": {
                    "device_properties": {
                      "disk_address": {}
                    }
                  }
                }
              }
            }
          },
          "os_type": "Linux",
          "type": "AHV_VM",
          "create_spec": {
            "backup_policy": null,
            "type": "",
            "name": "MongoRouterVM",
            "resources": {
              "nic_list": [
                {
                  "nic_type": "NORMAL_NIC",
                  "ip_endpoint_list": [],
                  "network_function_chain_reference": null,
                  "network_function_nic_type": "INGRESS",
                  "mac_address": "",
                  "subnet_reference": {
                    "kind": "subnet",
                    "type": "",
                    "name": "",
                    "uuid": "a6bb0e0b-fcb0-424c-95b3-5c700d11bdb8"
                  },
                  "type": ""
                }
              ],
              "parent_reference": null,
              "guest_tools": null,
              "num_vcpus_per_socket": 1,
              "num_sockets": 2,
              "gpu_list": [],
              "memory_size_mib": 4096,
              "power_state": "ON",
              "hardware_clock_timezone": "",
              "guest_customization": null,
              "type": "",
              "boot_config": {
                "boot_device": {
                  "type": "",
                  "disk_address": {
                    "adapter_type": "SCSI",
                    "device_index": 0,
                    "type": ""
                  }
                },
                "type": "",
                "mac_address": ""
              },
              "disk_list": [
                {
                  "data_source_reference": {
                    "kind": "image",
                    "type": "",
                    "name": "CentOS Server v7",
                    "uuid": "cb241054-89c4-40c4-8872-dcdbf7fa104d"
                  },
                  "type": "",
                  "disk_size_mib": 0,
                  "device_properties": {
                    "type": "",
                    "device_type": "DISK",
                    "disk_address": {
                      "adapter_type": "SCSI",
                      "device_index": 0,
                      "type": ""
                    }
                  }
                }
              ]
            },
            "availability_zone_reference": null
          },
          "variable_list": [],
          "name": "AHVRouter"
        },
        {
          "description": "",
          "action_list": [],
          "readiness_probe": {
            "connection_type": "SSH",
            "disable_readiness_probe": false,
            "timeout_secs": "60",
            "address": "@@{platform.status.resources.nic_list[0].ip_endpoint_list[0].ip}@@",
            "connection_port": 22,
            "login_credential_local_reference": {
              "kind": "app_credential",
              "name": "CENTOS"
            }
          },
          "editables": {
            "readiness_probe": {
              "connection_port": true,
              "timeout_secs": true
            },
            "create_spec": {
              "name": true,
              "resources": {
                "nic_list": {
                  "0": {
                    "subnet_reference": true
                  }
                },
                "num_vcpus_per_socket": true,
                "num_sockets": true,
                "memory_size_mib": true,
                "guest_customization": true,
                "disk_list": {
                  "0": {
                    "device_properties": {
                      "disk_address": {}
                    }
                  }
                }
              }
            }
          },
          "os_type": "Linux",
          "type": "AHV_VM",
          "create_spec": {
            "backup_policy": null,
            "type": "",
            "name": "MongoConfigSetVM",
            "resources": {
              "nic_list": [
                {
                  "nic_type": "NORMAL_NIC",
                  "ip_endpoint_list": [],
                  "network_function_chain_reference": null,
                  "network_function_nic_type": "INGRESS",
                  "mac_address": "",
                  "subnet_reference": {
                    "kind": "subnet",
                    "type": "",
                    "name": "",
                    "uuid": "a6bb0e0b-fcb0-424c-95b3-5c700d11bdb8"
                  },
                  "type": ""
                }
              ],
              "parent_reference": null,
              "guest_tools": null,
              "num_vcpus_per_socket": 1,
              "num_sockets": 2,
              "gpu_list": [],
              "memory_size_mib": 4096,
              "power_state": "ON",
              "hardware_clock_timezone": "",
              "guest_customization": null,
              "type": "",
              "boot_config": null,
              "disk_list": [
                {
                  "data_source_reference": {
                    "kind": "image",
                    "type": "",
                    "name": "CentOS Server v7",
                    "uuid": "cb241054-89c4-40c4-8872-dcdbf7fa104d"
                  },
                  "type": "",
                  "disk_size_mib": 0,
                  "device_properties": {
                    "type": "",
                    "device_type": "DISK",
                    "disk_address": {
                      "adapter_type": "SCSI",
                      "device_index": 0,
                      "type": ""
                    }
                  }
                }
              ]
            },
            "availability_zone_reference": null
          },
          "variable_list": [],
          "name": "AHVConfigSet"
        },
        {
          "description": "",
          "action_list": [],
          "readiness_probe": {
            "connection_type": "SSH",
            "disable_readiness_probe": false,
            "timeout_secs": "60",
            "address": "@@{platform.status.resources.nic_list[0].ip_endpoint_list[0].ip}@@",
            "connection_port": 22,
            "login_credential_local_reference": {
              "kind": "app_credential",
              "name": "CENTOS"
            }
          },
          "editables": {
            "readiness_probe": {
              "connection_port": true,
              "timeout_secs": true
            },
            "create_spec": {
              "name": true,
              "resources": {
                "nic_list": {
                  "0": {
                    "subnet_reference": true
                  }
                },
                "num_vcpus_per_socket": true,
                "num_sockets": true,
                "memory_size_mib": true,
                "guest_customization": true,
                "disk_list": {
                  "0": {
                    "device_properties": {
                      "disk_address": {}
                    }
                  }
                }
              }
            }
          },
          "os_type": "Linux",
          "type": "AHV_VM",
          "create_spec": {
            "backup_policy": null,
            "type": "",
            "name": "MongoReplicaSetVM",
            "resources": {
              "nic_list": [
                {
                  "nic_type": "NORMAL_NIC",
                  "ip_endpoint_list": [],
                  "network_function_chain_reference": null,
                  "network_function_nic_type": "INGRESS",
                  "mac_address": "",
                  "subnet_reference": {
                    "kind": "subnet",
                    "type": "",
                    "name": "",
                    "uuid": "a6bb0e0b-fcb0-424c-95b3-5c700d11bdb8"
                  },
                  "type": ""
                }
              ],
              "parent_reference": null,
              "guest_tools": null,
              "num_vcpus_per_socket": 1,
              "num_sockets": 2,
              "gpu_list": [],
              "memory_size_mib": 4096,
              "power_state": "ON",
              "hardware_clock_timezone": "",
              "guest_customization": null,
              "type": "",
              "boot_config": {
                "boot_device": {
                  "type": "",
                  "disk_address": {
                    "adapter_type": "SCSI",
                    "device_index": 0,
                    "type": ""
                  }
                },
                "type": "",
                "mac_address": ""
              },
              "disk_list": [
                {
                  "data_source_reference": {
                    "kind": "image",
                    "type": "",
                    "name": "CentOS Server v7",
                    "uuid": "cb241054-89c4-40c4-8872-dcdbf7fa104d"
                  },
                  "type": "",
                  "disk_size_mib": 0,
                  "device_properties": {
                    "type": "",
                    "device_type": "DISK",
                    "disk_address": {
                      "adapter_type": "SCSI",
                      "device_index": 0,
                      "type": ""
                    }
                  }
                }
              ]
            },
            "availability_zone_reference": null
          },
          "variable_list": [],
          "name": "AHVReplicaSet"
        }
      ],
      "credential_definition_list": [
        {
          "username": "root",
          "description": "",
          "secret": {
            "attrs": {
              "is_secret_modified": false,
              "secret_reference": {}
            }
          },
          "editables": {
            "username": true,
            "secret": true
          },
          "type": "PASSWORD",
          "name": "CENTOS"
        }
      ],
      "package_definition_list": [
        {
          "description": "",
          "action_list": [],
          "service_local_reference_list": [
            {
              "kind": "app_service",
              "name": "Mongo_Router"
            }
          ],
          "version": "",
          "type": "DEB",
          "options": {
            "install_runbook": {
              "variable_list": [],
              "task_definition_list": [
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVRouterPackage"
                  },
                  "description": "",
                  "child_tasks_local_reference_list": [
                    {
                      "kind": "app_task",
                      "name": "PackageInstallTask"
                    }
                  ],
                  "attrs": {
                    "edges": [],
                    "type": ""
                  },
                  "timeout_secs": "",
                  "type": "DAG",
                  "variable_list": [],
                  "name": "feb88e42_dag"
                },
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVRouterPackage"
                  },
                  "description": "",
                  "child_tasks_local_reference_list": [],
                  "attrs": {
                    "exit_status": [],
                    "script": "#!/bin/bash\nset -ex\nif [[ -f /sys/kernel/mm/transparent_hugepage/enabled ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled\nfi\nif [[ -f /sys/kernel/mm/transparent_hugepage/defrag ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag\nfi\n\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/sysconfig/selinux\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config\nsudo setenforce 0\n\nVERSION=\"3.4.2\"\nDB_PATH=\"@@{DB_PATH}@@\"\nDB_PATH=${DB_PATH:=\"/opt/mongodb\"}\nCONFIG_SRV_IPS=\"@@{Mongo_ConfigSet.address}@@\"\n\nsudo mkdir -p ${DB_PATH}\n\n\nconfig_ips=$(echo \"${CONFIG_SRV_IPS}\" | sed 's/^,//' | sed 's/,$//' | sed 's/,/:37019,/g')\nconfig_ips=$config_ips\":37019\"\n\necho '[mongodb-org-3.4]\nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/\ngpgcheck=1\nenabled=1\ngpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc' | sudo tee /etc/yum.repos.d/mongodb-3.4.repo\n\n# Add mongodb repository\nsudo yum update -y\n\n# Install mongodb packages\nsudo yum install -y mongodb-org-${VERSION} mongodb-org-server-${VERSION} mongodb-org-shell-${VERSION} mongodb-org-mongos-${VERSION} mongodb-org-tools-${VERSION}\n\n# Make sure mongodb does not get updated automatically\necho 'exclude=mongodb-org*' | sudo tee -a /etc/yum.conf\n\n# Prepare the path for the databases\nsudo chown mongod:mongod ${DB_PATH}\n\n# Enable mongodb to listen for remote connections\nsudo sed -i 's/bindIp:/#bindIp:/g' /etc/mongod.conf\nsudo sed -i \"s#/var/lib/mongo#${DB_PATH}#g\" /etc/mongod.conf\n\nsudo -u mongod -b mongos --port 27017 --configdb configReplSet/$config_ips  --logpath /var/log/mongodb/mongos.log \n\nsleep 2\n",
                    "script_type": "sh",
                    "type": "",
                    "command_line_args": "",
                    "login_credential_local_reference": {
                      "kind": "app_credential",
                      "name": "CENTOS"
                    }
                  },
                  "timeout_secs": "",
                  "type": "EXEC",
                  "variable_list": [],
                  "name": "PackageInstallTask"
                }
              ],
              "description": "",
              "main_task_local_reference": {
                "kind": "app_task",
                "name": "feb88e42_dag"
              },
              "name": "f2eb973b_runbook"
            },
            "type": "",
            "uninstall_runbook": {
              "variable_list": [],
              "task_definition_list": [
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVRouterPackage"
                  },
                  "description": "",
                  "child_tasks_local_reference_list": [
                    {
                      "kind": "app_task",
                      "name": "PackageUninstallTask"
                    }
                  ],
                  "attrs": {
                    "edges": [],
                    "type": ""
                  },
                  "timeout_secs": "",
                  "type": "DAG",
                  "variable_list": [],
                  "name": "db8d17a7_dag"
                },
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVRouterPackage"
                  },
                  "description": "",
                  "child_tasks_local_reference_list": [],
                  "attrs": {
                    "exit_status": [],
                    "script": "#!/bin/bash",
                    "script_type": "sh",
                    "type": "",
                    "command_line_args": "",
                    "login_credential_local_reference": {
                      "kind": "app_credential",
                      "name": "CENTOS"
                    }
                  },
                  "timeout_secs": "",
                  "type": "EXEC",
                  "variable_list": [],
                  "name": "PackageUninstallTask"
                }
              ],
              "description": "",
              "main_task_local_reference": {
                "kind": "app_task",
                "name": "db8d17a7_dag"
              },
              "name": "cf93cd7c_runbook"
            }
          },
          "variable_list": [],
          "name": "AHVRouterPackage"
        },
        {
          "description": "",
          "action_list": [],
          "service_local_reference_list": [
            {
              "kind": "app_service",
              "name": "Mongo_ConfigSet"
            }
          ],
          "version": "",
          "type": "DEB",
          "options": {
            "install_runbook": {
              "variable_list": [],
              "task_definition_list": [
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVConfigSetPackage"
                  },
                  "description": "",
                  "child_tasks_local_reference_list": [
                    {
                      "kind": "app_task",
                      "name": "PackageInstallTask"
                    }
                  ],
                  "attrs": {
                    "edges": [],
                    "type": ""
                  },
                  "timeout_secs": "",
                  "type": "DAG",
                  "variable_list": [],
                  "name": "40b53b04_dag"
                },
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVConfigSetPackage"
                  },
                  "description": "",
                  "child_tasks_local_reference_list": [],
                  "attrs": {
                    "exit_status": [],
                    "script": "#!/bin/bash\nset -ex\nif [[ -f /sys/kernel/mm/transparent_hugepage/enabled ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled\nfi\nif [[ -f /sys/kernel/mm/transparent_hugepage/defrag ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag\nfi\n\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/sysconfig/selinux\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config\nsudo setenforce 0\n\nVERSION=\"3.4.2\"\nDB_PATH=\"@@{DB_PATH}@@\"\nDB_PATH=${DB_PATH:=\"/opt/mongodb\"}\nCONFIG_SRV_IPS=\"@@{calm_array_address}@@\"\nHOST_IP=\"@@{address}@@\"\n\nsudo mkdir -p ${DB_PATH}\n\necho '[mongodb-org-3.4]\nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/\ngpgcheck=1\nenabled=1\ngpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc' | sudo tee /etc/yum.repos.d/mongodb-3.4.repo\n\n# Add mongodb repository\nsudo yum update -y\n\n# Install mongodb packages\nsudo yum install -y mongodb-org-${VERSION} mongodb-org-server-${VERSION} mongodb-org-shell-${VERSION} mongodb-org-mongos-${VERSION} mongodb-org-tools-${VERSION}\n\n# Make sure mongodb does not get updated automatically\necho 'exclude=mongodb-org*' | sudo tee -a /etc/yum.conf\n\n# Prepare the path for the databases\nsudo chown mongod:mongod ${DB_PATH}\n\n# Enable mongodb to listen for remote connections\nsudo sed -i 's/bindIp:/#bindIp:/g' /etc/mongod.conf\nsudo sed -i \"s#/var/lib/mongo#${DB_PATH}#g\" /etc/mongod.conf\nsudo sed -i 's/27017/37019/g' /etc/mongod.conf\nsudo sed -i 's/#sharding:/sharding:\\n  clusterRole: configsvr/g' /etc/mongod.conf\nsudo sed -i 's/#replication:/replication:\\n  replSetName: configReplSet/g' /etc/mongod.conf\n\n\nconfig_ips=$(echo \"${CONFIG_SRV_IPS}\" | sed 's/^,//' | sed 's/,$//' | tr \",\" \"\\n\")\nconfig_cnt=0\nconfig_str=\"\"\n\nfor config in $config_ips\ndo\n  config_str=$config_str\"{\\\"_id\\\": $config_cnt, host:\\\"$config:37019\\\"},\"\n  config_cnt=$(($config_cnt + 1))\ndone\nconfig_str=`echo $config_str | sed 's/,$//'`\njson=$(cat <<EOF\n{\n  \"_id\": \"configReplSet\",\n  \"members\": [\n    $config_str\n  ]\n}\nEOF\n)\n\nsudo systemctl restart mongod\nsleep 2\n\nsudo mongo --host ${HOST_IP} --port 37019 --eval \"rs.initiate( $json )\"\n\n",
                    "script_type": "sh",
                    "type": "",
                    "command_line_args": "",
                    "login_credential_local_reference": {
                      "kind": "app_credential",
                      "name": "CENTOS"
                    }
                  },
                  "timeout_secs": "",
                  "type": "EXEC",
                  "variable_list": [],
                  "name": "PackageInstallTask"
                }
              ],
              "description": "",
              "main_task_local_reference": {
                "kind": "app_task",
                "name": "40b53b04_dag"
              },
              "name": "fb351d28_runbook"
            },
            "type": "",
            "uninstall_runbook": {
              "variable_list": [],
              "task_definition_list": [
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVConfigSetPackage"
                  },
                  "description": "",
                  "child_tasks_local_reference_list": [
                    {
                      "kind": "app_task",
                      "name": "PackageUninstallTask"
                    }
                  ],
                  "attrs": {
                    "edges": [],
                    "type": ""
                  },
                  "timeout_secs": "",
                  "type": "DAG",
                  "variable_list": [],
                  "name": "87329188_dag"
                },
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVConfigSetPackage"
                  },
                  "description": "",
                  "child_tasks_local_reference_list": [],
                  "attrs": {
                    "exit_status": [],
                    "script": "#!/bin/bash",
                    "script_type": "sh",
                    "type": "",
                    "command_line_args": "",
                    "login_credential_local_reference": {
                      "kind": "app_credential",
                      "name": "CENTOS"
                    }
                  },
                  "timeout_secs": "",
                  "type": "EXEC",
                  "variable_list": [],
                  "name": "PackageUninstallTask"
                }
              ],
              "description": "",
              "main_task_local_reference": {
                "kind": "app_task",
                "name": "87329188_dag"
              },
              "name": "d338a852_runbook"
            }
          },
          "variable_list": [],
          "name": "AHVConfigSetPackage"
        },
        {
          "description": "",
          "action_list": [],
          "service_local_reference_list": [
            {
              "kind": "app_service",
              "name": "Mongo_ReplicaSet"
            }
          ],
          "version": "",
          "type": "DEB",
          "options": {
            "install_runbook": {
              "variable_list": [],
              "task_definition_list": [
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVReplicaSetPackage"
                  },
                  "description": "",
                  "child_tasks_local_reference_list": [
                    {
                      "kind": "app_task",
                      "name": "PackageInstallTask"
                    }
                  ],
                  "attrs": {
                    "edges": [],
                    "type": ""
                  },
                  "timeout_secs": "",
                  "type": "DAG",
                  "variable_list": [],
                  "name": "21686055_dag"
                },
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVReplicaSetPackage"
                  },
                  "description": "",
                  "child_tasks_local_reference_list": [],
                  "attrs": {
                    "exit_status": [],
                    "script": "#!/bin/bash\nset -ex\nif [[ -f /sys/kernel/mm/transparent_hugepage/enabled ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled\nfi\nif [[ -f /sys/kernel/mm/transparent_hugepage/defrag ]];then\n\techo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag\nfi\n\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/sysconfig/selinux\nsudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config\nsudo setenforce 0\n\nVERSION=\"3.4.2\"\nDB_PATH=\"@@{DB_PATH}@@\"\nDB_PATH=${DB_PATH:=\"/opt/mongodb\"}\nHOST_IP=\"@@{address}@@\"\narray_index=@@{calm_array_index}@@\nNODES_PER_REPLICA_SET=@@{NODES_PER_REPLICA_SET}@@\nreplica_ips=$(echo @@{calm_array_address}@@ | sed 's/^,//' | sed 's/,$//' | tr \",\" \"\\n\")\n\nsudo mkdir -p ${DB_PATH}\n\necho '[mongodb-org-3.4]\nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/\ngpgcheck=1\nenabled=1\ngpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc' | sudo tee /etc/yum.repos.d/mongodb-3.4.repo\n\n# Add mongodb repository\nsudo yum update -y\n\n# Install mongodb packages\nsudo yum install -y mongodb-org-${VERSION} mongodb-org-server-${VERSION} mongodb-org-shell-${VERSION} mongodb-org-mongos-${VERSION} mongodb-org-tools-${VERSION}\n\n# Make sure mongodb does not get updated automatically\necho 'exclude=mongodb-org*' | sudo tee -a /etc/yum.conf\n\n# Prepare the path for the databases\nsudo chown mongod:mongod ${DB_PATH}\n\n# Enable mongodb to listen for remote connections\ntmp=$((${array_index}+${NODES_PER_REPLICA_SET}))\nreplicasetNo=$((${tmp}/${NODES_PER_REPLICA_SET}))\nreplicaSetName=\"dataReplSet${replicasetNo}\"\n\nsudo sed -i 's/bindIp:/#bindIp:/g' /etc/mongod.conf\nsudo sed -i \"s#/var/lib/mongo#${DB_PATH}#g\" /etc/mongod.conf\nsudo sed -i \"s/#replication:/replication:\\n  replSetName: $replicaSetName/g\" /etc/mongod.conf\nsudo sed -i 's/#sharding:/sharding:\\n  clusterRole: shardsvr/g' /etc/mongod.conf\n\n### Getting replicaset members details \nreplica_cnt=0\ndeclare -a replicaset_members\n\nfor replica in $replica_ips\ndo\n  tmp=$((${replica_cnt}+${NODES_PER_REPLICA_SET}))\n  i=\"$((${tmp}/${NODES_PER_REPLICA_SET}))\"\n  replicaset_members[$i]=${replicaset_members[$i]}\"{\\\"_id\\\": $replica_cnt, host:\\\"$replica:27017\\\"},\"\n  replica_cnt=$(($replica_cnt + 1))\ndone\n\nsudo systemctl restart mongod\nsleep 2\n\n### Initiating replicaset \njson=$(cat <<EOF\n{\n  \"_id\": \"$replicaSetName\",\n  \"members\": [ ${replicaset_members[$replicasetNo]}]\n}\nEOF\n)\nsudo mongo --host ${HOST_IP} --port 27017 --eval \"rs.initiate( $json )\"\nsleep 2\n",
                    "script_type": "sh",
                    "type": "",
                    "command_line_args": "",
                    "login_credential_local_reference": {
                      "kind": "app_credential",
                      "name": "CENTOS"
                    }
                  },
                  "timeout_secs": "",
                  "type": "EXEC",
                  "variable_list": [],
                  "name": "PackageInstallTask"
                }
              ],
              "description": "",
              "main_task_local_reference": {
                "kind": "app_task",
                "name": "21686055_dag"
              },
              "name": "0aecfbfc_runbook"
            },
            "type": "",
            "uninstall_runbook": {
              "variable_list": [],
              "task_definition_list": [
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVReplicaSetPackage"
                  },
                  "description": "",
                  "child_tasks_local_reference_list": [
                    {
                      "kind": "app_task",
                      "name": "PackageUninstallTask"
                    }
                  ],
                  "attrs": {
                    "edges": [],
                    "type": ""
                  },
                  "timeout_secs": "",
                  "type": "DAG",
                  "variable_list": [],
                  "name": "46111458_dag"
                },
                {
                  "target_any_local_reference": {
                    "kind": "app_package",
                    "name": "AHVReplicaSetPackage"
                  },
                  "description": "",
                  "child_tasks_local_reference_list": [],
                  "attrs": {
                    "exit_status": [],
                    "script": "#!/bin/bash",
                    "script_type": "sh",
                    "type": "",
                    "command_line_args": "",
                    "login_credential_local_reference": {
                      "kind": "app_credential",
                      "name": "CENTOS"
                    }
                  },
                  "timeout_secs": "",
                  "type": "EXEC",
                  "variable_list": [],
                  "name": "PackageUninstallTask"
                }
              ],
              "description": "",
              "main_task_local_reference": {
                "kind": "app_task",
                "name": "46111458_dag"
              },
              "name": "d38a2e0d_runbook"
            }
          },
          "variable_list": [],
          "name": "AHVReplicaSetPackage"
        },
        {
          "description": "",
          "action_list": [],
          "service_local_reference_list": [],
          "version": "",
          "type": "SUBSTRATE_IMAGE",
          "options": {
            "type": "",
            "name": "CentOs-7.4-Mongo-ConfigSet",
            "resources": {
              "image_type": "DISK_IMAGE",
              "checksum": {
                "checksum_algorithm": "",
                "type": "",
                "checksum_value": ""
              },
              "source_uri": "http://download.nutanix.com/calm/CentOS-7-x86_64-GenericCloud.qcow2",
              "version": {
                "product_version": "",
                "type": "",
                "product_name": ""
              },
              "architecture": "X86_64",
              "type": ""
            },
            "description": ""
          },
          "variable_list": [],
          "name": "CENTOS_ConfigSet"
        },
        {
          "description": "",
          "action_list": [],
          "service_local_reference_list": [],
          "version": "",
          "type": "SUBSTRATE_IMAGE",
          "options": {
            "type": "",
            "name": "CentOs-7.4-Mongo-ReplicaSet",
            "resources": {
              "image_type": "DISK_IMAGE",
              "checksum": {
                "checksum_algorithm": "",
                "type": "",
                "checksum_value": ""
              },
              "source_uri": "http://download.nutanix.com/calm/CentOS-7-x86_64-GenericCloud.qcow2",
              "version": {
                "product_version": "",
                "type": "",
                "product_name": ""
              },
              "architecture": "X86_64",
              "type": ""
            },
            "description": ""
          },
          "variable_list": [],
          "name": "CENTOS_ReplicaSet"
        },
        {
          "description": "",
          "action_list": [],
          "service_local_reference_list": [],
          "version": "",
          "type": "SUBSTRATE_IMAGE",
          "options": {
            "type": "",
            "name": "CentOs-7.4-Mongo-Router",
            "resources": {
              "image_type": "DISK_IMAGE",
              "checksum": {
                "checksum_algorithm": "",
                "type": "",
                "checksum_value": ""
              },
              "source_uri": "http://download.nutanix.com/calm/CentOS-7-x86_64-GenericCloud.qcow2",
              "version": {
                "product_version": "",
                "type": "",
                "product_name": ""
              },
              "architecture": "X86_64",
              "type": ""
            },
            "description": ""
          },
          "variable_list": [],
          "name": "CENTOS_Router"
        }
      ],
      "app_profile_list": [
        {
          "deployment_create_list": [
            {
              "description": "",
              "action_list": [],
              "package_local_reference_list": [
                {
                  "kind": "app_package",
                  "name": "AHVRouterPackage"
                }
              ],
              "max_replicas": "1",
              "substrate_local_reference": {
                "kind": "app_substrate",
                "name": "AHVRouter"
              },
              "min_replicas": "1",
              "variable_list": [],
              "name": "9b1e9738_deployment"
            },
            {
              "name": "e0823432_deployment",
              "action_list": [],
              "package_local_reference_list": [
                {
                  "kind": "app_package",
                  "name": "AHVConfigSetPackage"
                }
              ],
              "editables": {
                "min_replicas": true
              },
              "max_replicas": "1",
              "substrate_local_reference": {
                "kind": "app_substrate",
                "name": "AHVConfigSet"
              },
              "min_replicas": "1",
              "variable_list": [],
              "description": ""
            },
            {
              "name": "871434ae_deployment",
              "action_list": [],
              "package_local_reference_list": [
                {
                  "kind": "app_package",
                  "name": "AHVReplicaSetPackage"
                }
              ],
              "editables": {
                "min_replicas": true
              },
              "max_replicas": "2",
              "substrate_local_reference": {
                "kind": "app_substrate",
                "name": "AHVReplicaSet"
              },
              "min_replicas": "2",
              "variable_list": [],
              "description": ""
            }
          ],
          "variable_list": [
            {
              "val_type": "STRING",
              "description": "",
              "value": "/opt/mongodb",
              "label": "",
              "attrs": {
                "type": ""
              },
              "editables": {
                "value": true
              },
              "type": "LOCAL",
              "name": "DB_PATH"
            },
            {
              "val_type": "STRING",
              "description": "",
              "value": "2",
              "label": "",
              "attrs": {
                "type": ""
              },
              "editables": {
                "value": true
              },
              "type": "LOCAL",
              "name": "NODES_PER_REPLICA_SET"
            },
            {
              "val_type": "STRING",
              "description": "",
              "value": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDMGAS/K0Mb/uwFXwD+hkjguy7VMZk2hpuhwPl9FUZwVBrURf/i9QMJ5/paPEZixu8VlRx7Inu4iun7rQfrnfeIYInmBwspXHYiTK3oHJAgZnrAHVEf1p6YaxLINlT1NI5yOAGPRWW6of8rBDBH1ObwU2+wcSx/1H0uIs3aZNLufr+Rh628ACxAum2Gt8AVRj6ua2BPFyt5VTdclyysAmeh1AiixNgOZXOz6y/i4TbzpY78I3isuKpxsUeXX6jxEMQol406jHDUF6njEOPIQG2zVZ3QJlTG9OlN+NiyZG9WkZz0VG/6M8ixxIHHI2dNwUbBFv2HUu+8X9LTLFq2O7KjX9Hp7uZKBAySHA3eKaKHIp2bZuU1bT5PRPkggngX86xg+T+OMNnutbAiMnRJ8+FvD5So+5TIx4b9GgxAxure3x2yRPT9lOiQOB+CVpJPxR0Rn9bOI+wiPnD0kAGvK/fHT+pqL4PM+hTnJtp9rrCRzIQApBx1263jEcYffhW2epZQRO+he5CMawFJ5TBe08om2AaDJ8GQdrpF6YA3W8DzHbmL3DPVVHdmqPLn10o+LX4gv5SdIIDVGdjKOc1BCnLTRmM28d5+sLDt/M+kvcQgf0y0yDjMVjGECZkt39hbm4ELMHzZtzYLmHNhBZxRqHeJ7qFTuv1kx88OW3Xc5mbBNQ== centos@nutanix.com",
              "label": "",
              "attrs": {
                "type": ""
              },
              "editables": {
                "value": true
              },
              "type": "LOCAL",
              "name": "INSTANCE_PUBLIC_KEY"
            }
          ],
          "description": "",
          "action_list": [],
          "name": "Nutanix"
        }
      ],
      "default_credential_local_reference": {
        "kind": "app_credential",
        "name": "CENTOS"
      },
      "client_attrs": {
        "Mongo_ConfigSet": {
          "y": 40,
          "x": 140
        },
        "Mongo_Router": {
          "y": 40,
          "x": 360
        },
        "Mongo_ReplicaSet": {
          "y": 40,
          "x": 540
        }
      }
    },
    "name": "Import_API_Lab"
  },
  "api_version": "3.0",
  "metadata": {
    "last_update_time": "1514619292431733",
    "creation_time": "1514593937807713",
    "kind": "blueprint",
    "spec_version": 4,
    "name": "Import_API_Lab"
  }
}

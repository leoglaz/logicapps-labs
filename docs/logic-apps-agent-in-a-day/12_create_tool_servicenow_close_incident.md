---
title: 12 - Create the 'ServiceNow Close Incident' tool
description: Build a stateful workflow tool to close ServiceNow incidents with resolution notes.
ms.service: logic-apps
ms.topic: tutorial
ms.date: 08/19/2025
author: absaafan
ms.author: absaafan
---

In this module we will create a stateful workflow to close an existing ServiceNow incident and add resolution comments.

## Create the Stateful Workflow
1. Search for and navigate to the Logic Apps service

    ![Search and Navigate to the Logic Apps Service](./images/12_01_search_bar_logic_apps.png "search and navigate to logics apps")


1. Open the Logic App created earlier 

    ![Open Logic App](./images/12_02_logic_apps_list.png "open logic app")

1. Create a new workflow
    - Click `Workflows -> Workflows` from the menu on the left
    - Click `+ Add -> Add`

      ![Create New Workflow](./images/12_03_create_new_workflow.png "create new workflow")

1. Create a new stateful workflow with:
    
    - **Workflow name:** `tool-ServiceNow-CloseIncident`
    - Select the radio button for the `Stateful` workflow type
    - Click `Create`

    ![Create Stateful Workflow](./images/12_04_create_new_stateful_workflow.png "create new stateful workflow")

1. Open the workflow visual editor by clicking on the `tool-ServiceNow-UpdateIncident` link (if the workflow designer isn't opened automatically)

    ![Open Workflow](./images/12_05_open_workflow.png "Open Workflow" )

## Configure Workflow
1. Configure the workflow trigger to accept an HTTP Request
    - Click on `Add Trigger`
    - Select the `Request` action located in the **Built-in tools** group

        ![Add Trigger - Request Action](./images/12_06_add_trigger_request_action.png "add trigger request action")
        
    - Select the `When a HTTP request is received`

        ![Select Action When a HTTP Request is Recieved](./images/12_07_add_action_when_a_http_request_is_received.png "select when a HTTP 
        request is received")

1. Configure the `When a HTTP request is received` action:
    - **Request Body JSON Schema**
        ```JSON
       {
            "type": "object",
            "properties": {
                "TicketNumber": {
                    "type": "string"
                },
                "Notes": {
                    "type": "string"
                }
            }
       }
       ```

1. Look up the internal identifier for the **Incident** in ServiceNow

    - Add a new action. Click `+ Add an action`

        ![Add an action](./images/12_08_add_a_action.png "add a action")

    - Select the `ServiceNow - List Records` action

        ![Select Action ServiceNow List Records](./images/12_09_action_servicenow_list_records.png "servicenow list records")

1. Configure the List Records Activity as follows
    - **Record Type:** `Incident`
    - **Advanced Parameters** (click `Show all`)
    - **Query:** `number=@{triggerBody()?['TicketNumber']}`

        (**note:** notice that the connection for the ServiceNow connection was automatically selected for the activity)

        ![ServiceNow List Activity Configuration](./images/12_10_servicenow_list_records_config.png "servicenow list records configuration")

1. Add the **Update Record** action to update the work notes on the incident in ServiceNow
    - Click on the `+` -> `Add an Action`
    - Search for `ServiceNow` Connector and select the `Update Record` Activity

        ![ServiceNow Update Activity](./images/12_11_search_action_sevicenow_update_activity.png "servicenow update activity")

1. Configure the **Update Record** action
    - Rename activity to `Update Incident Work Notes`
    - **Record Type:** `Incident`
    - **System ID:** *(using the expression (fx) editor)* `first(body('List_Records')?['result'])['sys_id']`
    - **State:** *(Advanced Parameter)* `7`
    - **Resolution Code:** *(Advanced Parameter)* `Solution Provided`
    - **Resolution Notes:** *(Advanced Parameter)* `@{triggerBody()?['Notes']}`

        ![ServiceNow Update Activity Config](./images/12_12_update_activity_config.png "servicenow update activity config")

1. Add the **Response** activity to return a status message to the calling process
    - Click on the `+` -> `Add an Action`
    - Search for and select the `Response` activity

    ![Search Activity Response](./images/12_13_search_activity_response.png "search activity response")

1. Configure the **Response** activity
    - **Body:** 
        ```
        {
            "status": "Ticket {@{triggerBody()?['TicketNumber']}} has been updated successfully"
        }
        ```
    ![Response Activity Config](./images/12_14_response_activity_config.png "response activity config")

1. Save your workflow

    ![Save Workflow](./images/12_15_save_workflow.png "save workflow")


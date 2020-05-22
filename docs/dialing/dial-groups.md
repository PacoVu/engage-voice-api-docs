# About Dial Groups

Dial groups are configurable groups of (outbound) campaigns that can be differentiated by the type of dialer you are using. When you assign agents to one or more dial groups, those agents will have the ability to dial on all campaigns associated with those dial groups.

Once you’ve created a dial group, you can choose the appropriate dialer to apply to that group (Preview, Predictive, etc.) and configure a few settings that will apply to all the campaigns that get put into that dial group.

While you can create multiple dial groups — each of which can use a different dialer — please note that agents can only ever be actively logged into one dial group (and thus only using one outbound dialer) at a time.  You’ll notice that when you configure your agent login screens, you can give agents the option to make multiple concurrent choices from a list of inbound queues, but when it comes to outbound dialing, they will only be able to choose one dial group to dial on at any given time.

## Core Concepts

### Campaigns
Campaigns live within a dial group, which means you must set up and configure a dial group before you create your first campaign.  Dial groups provide a pool of campaigns an agent can log into and dial from.

### Dial Mode
Dial groups are differentiated by dial mode. Each dial group will only dial via one particular dialing mode at a time. Two dial modes in particular are important here: Predictive dialing and Preivew dialing.

-   **Predictive Dialing**

    Predictive dialing is when an auto dialer does the dialing for you. It automatically dials leads based on the settings configured in a predictive dial group.

    In addition to dialing faster and eliminating dialing mistakes due to human error, the predictive dialer lets the system balance the amount of calls an agent gets. It follows an intelligent predictive algorithm that learns and adjusts itself according to the patterns it detects.

    These predictions can then help the system decide a course of action, such as how many numbers it can simultaneously dial based on how many agents are available at any given time.

    As calls and agents fluctuate throughout calling hours, the predictive dialer will also adjust its algorithm continuously to match these changes.

-   **Preview Dialing**

    Preview dialing allows you to study lead information like call history, customer preferences, and notes on previous interactions before placing calls.

    Unlike predictive dialing where the system dials multiple numbers and assigns a call to an agent, preview dialing functions on a one-to-one basis where one lead is connected to one agent. This dialing mode ensures that you have time to view important information about each lead before a call is placed.

    Preview dialing is similar to progressive dialing, but the key difference between the two is that in progressive dialing, you can only study the lead info in a set amount of time configured by your administrators.

### Assigning Agents
If you’d like for your agent to be able to make outbound calls, you’ll need to assign that agent user to an outbound dial group.

!!! alert "Helpful hint"
    If you’d like your agents to make outbound calls, in addition to assigning your agent to an outbound dial group, you must also enable 'Allow Outbound Calls' via the [Agent](../../users/agents/agents).

## Creating a Dial Groups
Let's start with a new dial group to contain all our campaigns.

### Primary Parameters
Only `gateName` is a required parameter to create a Queue. All other parameters are optional.

| API Property |  | UI Display | UI Default | Description |
|-|-|-|-|-|
| **`isActive`** | Optional | Active | *checked* | Set this dial group to active or not. |
| **`dialGroupId`** | Optional | *hidden* | 0 | A unique dial group ID. By specfying 0, a unique ID will be chosen for you. |
| **`dialGroupName`** | Required | Name | *empty* | Give this dial group a name. |
| **`dialGroupDesc`** | Optional | Description | *empty* | Set a short description for the new dial group. |
| **`dialMode`** | Optional | Dial Mode | *empty* | Dial modes are typically one of two choices; `PREDICTIVE` or `PREVIEW`. |
| **Predictive Dialing Mode Settings** | | | | |
| **`enableAbsolutePriority`** | Optional | Enable Absolute Campaign Priority | *unchecked* | When this setting is enabled, the system will dial higher priority campaigns before any others as long as the higher priority campaigns still have active leads. |
| **Preview Dialing Mode Settings** | | | | |
| **`requireFetchedLeadsCalled`** | Optional | Require Calling of Fetched Leads | *unchecked* | Select this option to require agents to call all fetched leads before being able to fetch new leads. |
| **`progressiveEnabled`** | Optional | Enable Progressive Mode | *unchecked* | This mode will dial leads on a one-to-one, lead-to-agent basis where agents will be able to view the leads that will be dialed, but will not be able to choose which leads to dial or the order in which they are dialed. |
| **`progressiveCallDelay`** | Optional | Progressive Call Delay (seconds) | 0 | This setting is only used when the Enable Progressive Mode setting is enabled. This allows you to choose the time (in seconds) that the dialer will wait (once an agent has finished a call) before dialing the next lead. The maximum number of seconds you can choose is `120`. |
| **`allowPreviewLeadFilters`** | Optional | Preview Dial Lead Search | *unchecked* | When enabled, this option allows agents to use search filters when fetching leads to dial. Disabling this feature may improve lead fetch times. |
| **`maxLeadsReturned`** | Optional | Max Leads Returned | 1 | This setting indicates the maximum number of leads an agent can receive at a time (the agent can keep the leads for one hour). The maximum number of leads you can set is `50`. |
| **Common Mode Settings for both Predictive and Preview Modes** | | | | |
| **`enableAgentFilter`** | Optional | Enable Agent Filter | *unchecked* | This option is available when you enable Lead Search, or when using the Preview Dial Mode. When this option is enabled, the system will allow a lead with a reserved agent ID to be reserved for dialing only by the specified agent. Please note that this setting must be enabled in order for the Pending Agent ID and Reserve/Unreserve for Agent settings to function. |
| **`enableListPriority`** | Optional | Enable List Priority | *unchecked* | This option allows you to enable list dialing based on a custom priority. Once enabled, you can use the Priority setting in the Loaded Lists table on a campaign to select the dial list order. |
| **`allowLeadSearch`** | Optional | Allow Lead Search | *unchecked* | This option controls whether or not agents can search for leads when dialing from any campaign within this dial group. |
| **`enableCallbacksAfterMaxpass`** | Optional | Allow Scheduled Callbacks After Max Passes | *checked* | When enabled, this setting allows  agents to dial leads past the max pass limit (this limit can be set at the campaign level) as long as the last agent disposition selected was set to Requeue = Yes. If disabled, the lead will obey the max pass limit regardless of a request for a callback. |
| **`enableCallbacksAfterMaxDailyPass`** | Optional | Allow Scheduled Callbacks After Max Daily Passes | *unchecked* | When enabled, this setting allows agents to dial leads past the max daily pass limit (this limit can be set at the campaign level) as long as the last agent disposition selected was set to Requeue = Yes. If disabled, the lead will obey the max daily pass limit regardless of a request for a callback. |

### Request

```html tab="HTTP"
POST {BASE_URL}/api/v1/admin/accounts/{accountId}/dialGroups
Content-Type: application/json

{
  "dialGroupId": 0,
  "dialGroupName":"My Dial Group - Predictive",
  "dialGroupDesc":"A test dial group with predictive dial mode",
  "dialMode":"PREDICTIVE",
  "isActive": true,
}
```

```python tab="Python"
from engagevoice.sdk_wrapper import *

# Instantiate the SDK wrapper object with your RingCentral app credentials
ev = RestClient("RC_APP_CLIENT_ID", "RC_APP_CLIENT_SECRET")

# Login your account with your RingCentral Office user credentials
try:
    ev.login("RC_USERNAME", "RC_PASSWORD", "RC_EXTENSION_NUMBER")
    endpoint = 'admin/accounts/~/dialGroups'
    params = {
      "dialGroupId":0,
      "dialGroupName":"My Dial Group - Predictive",
      "dialGroupDesc":"A test dial group with predictive dial mode",
      "dialMode":"PREDICTIVE",
      "isActive":True
    }
    response = ev.post(endpoint, params)
    print (response)        
except Exception as e:
    print (e)
```

### Response

```json
{
  "isActive":true,
  "dialGroupId":115793,
  "dialGroupName":"My Dial Group - Predictive",
  "dialGroupDesc":"A test dial group with predictive dial mode",
  "dialMode":"PREDICTIVE",
  "enableAbsolutePriority":false,
  "enableAgentFilter":false,
  "requireFetchedLeadsCalled":false,
  "progressiveEnabled":false,
  "progressiveCallDelay":0,
  "allowPreviewLeadFilters":false,
  "maxLeadsReturned":1,
  "enableListPriority":false,
  "allowLeadSearch":"NO",
  "enableCallbacksAfterMaxpass":true,
  "enableCallbacksAfterMaxDailyPass":false,
  "minPredictiveAgents":1,
  "maxPorts":1,
  "agentsReady":0,
  "billingKey":null,
  "outdialServerGroupId":0,
  "realTimeAgentData":false,
  "hciEnabled":"DISABLED",
  "agentDialGroupMembers":[],
  "permissions":[],
}
```

## Retrieve a Single Dial Group

Now let's retrieve details for the Dial Group we just created to make to make sure it was created properly and to see what field were auto-populated with. This call will use the `dialGroups` endpoint.

### Request

```html tab="HTTP"
GET {BASE_URL}/api/v1/admin/accounts/{accountId}/dialGroups/{dialGroupId}
```

```python tab="Python"
from engagevoice.sdk_wrapper import *

# Instantiate the SDK wrapper object with your RingCentral app credentials
ev = RestClient("RC_APP_CLIENT_ID", "RC_APP_CLIENT_SECRET")

# Login your account with your RingCentral Office user credentials
try:
    ev.login("RC_USERNAME", "RC_PASSWORD", "RC_EXTENSION_NUMBER")
    endpoint = 'admin/accounts/~/dialGroups'
    resp = ev.get(endpoint)
    for group in resp:
    if (group['dialGroupName'] == "My Dial Group - Predictive"):
        endpoint = 'admin/accounts/~/dialGroups/%i' % (group['dialGroupId'])
        response = ev.get(endpoint)
        print (response))
except Exception as e:
    print (e)
```

### Response

```json
{
    "permissions": [],
    "dialGroupId": 115793,
    "dialGroupName": "My Dial Group - Predictive",
    "dialGroupDesc": "A test dial group with predictive dial mode",
    "dialMode": "PREDICTIVE",
    "minPredictiveAgents": 1,
    "enableAgentFilter": false,
    "maxPorts": 1,
    "isActive": true,
    "agentsReady": 0,
    "billingKey": null,
    "outdialServerGroupId": 0,
    "realTimeAgentData": false,
    "allowLeadSearch": "NO",
    "maxLeadsReturned": 1,
    "enableListPriority": false,
    "requireFetchedLeadsCalled": false,
    "allowPreviewLeadFilters": false,
    "progressiveEnabled": false,
    "progressiveCallDelay": 0,
    "enableCallbacksAfterMaxpass": true,
    "enableAbsolutePriority": false,
    "hciEnabled": "DISABLED",
    "enableCallbacksAfterMaxDailyPass": false,
    "agentDialGroupMembers": null
}
```

## Update a Single Dial Group

Note the `dialGroupId`. We will use that ID to update the Dial Group we created.  Let's say we want to now enable all the settings for the Predictive Dial Group using the `dialGroups` endpoint.

### Request

```html tab="HTTP" hl_lines="11 12 13 14 15 16"
PUT {BASE_URL}/api/v1/admin/accounts/{accountId}/dialGroups/{dialGroupId}
{
  "permissions": [],
  "dialGroupId": 115793,
  "dialGroupName":"My Dial Group - Predictive",
  "dialGroupDesc":"A test dial group with predictive dial mode",
  "dialMode":"PREDICTIVE",
  "isActive": true,
  "hciEnabled": "DISABLED",
  "agentDialGroupMembers": null,
  "enableAbsolutePriority":true,
  "enableAgentFilter":true,
  "enableListPriority":true,
  "allowLeadSearch":"YES",
  "enableCallbacksAfterMaxpass":true,
  "enableCallbacksAfterMaxDailyPass":true
}
```

```python tab="Python" hl_lines="19 20 21 22 23 24"
from engagevoice.sdk_wrapper import *

# Instantiate the SDK wrapper object with your RingCentral app credentials
ev = RestClient("RC_APP_CLIENT_ID", "RC_APP_CLIENT_SECRET")

# Login your account with your RingCentral Office user credentials
try:
    ev.login("RC_USERNAME", "RC_PASSWORD", "RC_EXTENSION_NUMBER")
    endpoint = 'admin/accounts/~/dialGroups/'
    params = {
      "permissions":[],
      "dialGroupId":115793,
      "dialGroupName":"My Dial Group - Predictive",
      "dialGroupDesc":"A test dial group with predictive dial mode",
      "dialMode":"PREDICTIVE",
      "isActive":True,
      "hciEnabled": "DISABLED",
      "agentDialGroupMembers":None,
      "enableAbsolutePriority":True,
      "enableAgentFilter":True,
      "enableListPriority":True,
      "allowLeadSearch":"YES",
      "enableCallbacksAfterMaxpass":True,
      "enableCallbacksAfterMaxDailyPass":True
    }
    resp = ev.get(endpoint)
    for group in resp:
        if (group['dialGroupName'] == "My Dial Group - Predictive"):
            endpoint = 'admin/accounts/~/dialGroups/%i' % (group['dialGroupId'])
            response = ev.put(endpoint, params)
            print (response)
except Exception as e:
    print (e)
```

### Response

```json
{
    "permissions": [],
    "dialGroupId": 115793,
    "dialGroupName": "My Dial Group - Predictive",
    "dialGroupDesc": "A test dial group with predictive dial mode",
    "dialMode": "PREDICTIVE",
    "minPredictiveAgents": 1,
    "enableAgentFilter": true,
    "maxPorts": 1,
    "isActive": true,
    "agentsReady": 0,
    "billingKey": null,
    "outdialServerGroupId": 0,
    "realTimeAgentData": false,
    "allowLeadSearch": "YES",
    "maxLeadsReturned": 1,
    "enableListPriority": true,
    "requireFetchedLeadsCalled": false,
    "allowPreviewLeadFilters": false,
    "progressiveEnabled": false,
    "progressiveCallDelay": 0,
    "enableCallbacksAfterMaxpass": true,
    "enableAbsolutePriority": true,
    "hciEnabled": "DISABLED",
    "enableCallbacksAfterMaxDailyPass": true,
    "agentDialGroupMembers": null
}
```

And now we have a Dial Group configured with all our information settings configured.
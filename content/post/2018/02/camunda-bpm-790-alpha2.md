+++
author = "Fabian Hinsenkamp"
categories = ["Execution"]
tags = ["Release Note"]
date = "2018-03-02T12:00:00+01:00"
title = "Camunda 7.9.0-alpha2 Released"
+++

The second alpha version of Camunda 7.9.0 is here and it is loaded with new features. The highlights are:


* Long polling for 'Fetch and Lock' External Tasks
* Throttle login attempts
* Additional filtering options for 'Fetch and Lock'
* Support for expressions in External Task topics

<!--more-->

## Long Polling for 'Fetch and Lock' External Tasks

Ordinary HTTP requests are immediately answered by the server, irrespective of whether or not the requested information 
is available. This inevitably leads to a situation where the client has to perform multiple recurring requests until 
the information is available (polling). This approach can obviously be expensive in terms of resources.

We tackle this bottleneck by introducing **long polling** to 'Fetch and Lock' External Tasks.

{{< figure class="teaser no-border" src="external-task-long-polling.png" alt="Long polling to 'Fetch and Lock' External Tasks" >}}

The basic idea behind long polling is, that a request is suspended by the server if no external tasks are available. As soon as new 
external tasks occur, the request is reactivated and the response is performed. The suspension is limited to a 
configurable period of time (timeout).

Long polling significantly reduces the number of requests and enables using resources more efficiently on both,   
the server and the client side.

The following example shows how to perform a long polling request:

**POST** `/engine-rest/external-task/fetchAndLock`

```javascript
{
  "asyncResponseTimeout": 1000,
  "workerId": "workerOne",
  "topics": [
    {
      "topicName": "validateAddress",
      "lockDuration": 10000
    }
  ]
}
```

## Throttle login attempts

Within this alpha release we introduce a mechanism for throttling login attempts.
After each unsuccessful attempt the user needs to wait for specified time until next attempt.
This delay is configurable and you can find more information about it in the [User guide](https://docs.camunda.org/manual/latest/user-guide/process-engine/identity-service/#throttle-login-attempts).
The user will be locked (not able to login) after reaching predefined maximum number of unsuccessful attempts to login. (Bear in mind that only a Camunda administrator is able to unlock users.)

This mechanism is not applicable if the system uses LDAP configuration. Each LDAP vendor offers similar mechanisms allowing you to achieve the same behaviour. 

## Additional filtering options for Fetch and Lock

When the client has already received the queried External Tasks, resources may be further expended by trying to confirm that the fetched tasks conform to the client's criteria.
By providing additional filtering options to the 'Fetch and Lock' feature for External Tasks, a more fine-grained control is obtained over which External Tasks are fetched and locked.

### Filter External Task topics by Business Key

When filtering External Task topics by Business Key, it can be ensured that an External Task, associated with a certain topic, also has the defined Business Key.

Here's an example of how to utilise this feature:

**POST** `/engine-rest/external-task/fetchAndLock`

```javascript
{
  "workerId":"aWorkerId",
  "maxTasks":2,
  "usePriority":true,
  "topics":[
    {
      "topicName": "createOrder",
      "businessKey": "aBusinessKey",
      "lockDuration": 10000,
      "variables": ["orderId"]
    }
  ]
}
```

### Filter External Task topics by Process Variable values

Another filter that can be applied allows to filter External Task topics by Process Variable values.

Below is an example on how this feature can be utilised:

**POST** `/engine-rest/external-task/fetchAndLock`

```javascript
{
  "workerId":"aWorkerId",
  "maxTasks":2,
  "usePriority":true,
  "topics":[
    {
      "topicName": "createOrder",
      "processVariables": {
        "variableName": "variableValue",
        "anotherVariableName": "anotherVariableValue"
      },
      "lockDuration": 10000,
      "variables": ["orderId"]
    }
  ]
}
```

## Support for expressions in External Task topics

Now possible to dynamically determine an External Task's topic during runtime by providing expression support in the External Task topic. 
This allows more flexible topic assignment and a wider range of application scenarios.

Here is one usage scenario of this new feature:

```xml
  <!-- where invoiceType may be "incoming" or "outgoing" -->
  <serviceTask
    id="externalTask"
    name="externalTask"
    camunda:type="external"
    camunda:topic="${invoiceType}" />
```

## Feature 4



## What's Next?
The next alpha version is scheduled for the end of March and our team is already working on it.

Here are few highlights if you want to know what the team is preparing for the next releases:

* Client for External Tasks in JavaScript and Java  

You can also find out more details if you check out our [roadmap](https://camunda.com/learn/community/#roadmap).

## Your Feedback Matters!
Your feedback is extremely important for us in order to improve Camunda BPM, so your thoughts are always highly appreciated and considered by our team.

Feel free to share your ideas and suggestions with us by writing a post in the [forum](https://forum.camunda.org/).

Furthermore, if you have any feedback related to User Experience, things that keep bugging you, things that you think should work differently etc., please share your thoughts with us at [https://camundabpm.userecho.com](https://camundabpm.userecho.com)

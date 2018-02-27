+++
author = "Fabian Hinsenkamp"
categories = ["Execution"]
tags = ["Release Note"]
date = "2018-03-02T12:00:00+01:00"
title = "Camunda 7.9.0-alpha2 Released"
+++

The second alpha version of Camunda 7.9.0 is here and it is loaded with new features. The highlights are:


* Long polling to fetch and lock external tasks
* Throttle login attempts
* Feature 1 
* Feature 2

<!--more-->

## Long Polling to Fetch and Lock External Tasks

Ordinary HTTP requests are immediately answered by the server, irrespective of whether or not the requested information 
is available. This inevitably leads to a situation where the client has to perform multiple recurring requests until 
the information is available (polling). This approach can obviously be expensive in terms of resources.

We tackle this bottleneck by introducing **long polling** to fetch and lock external tasks.

{{< figure class="teaser no-border" src="external-task-long-polling.png" alt="Long polling to fetch and lock external tasks" >}}

With the aid of long polling a request is suspended by the server if no external tasks are available. As soon as new 
external tasks occur, the request is reactivated and the response is performed. The suspension is limited to a 
configurable period of time (timeout).

Long polling significantly reduces the number of requests and enables using resources more efficiently on both 
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

## Trottle login attempts

Within this alpha release we introduce a mechanism for throttling login attempts.
After each unsuccessful attempt the user needs to wait for specified time until next attempt.
This delay is configurable and you can find more information about it in the [User guide](https://docs.camunda.org/manual/latest/user-guide/process-engine/identity-service/#throttle-login-attempts).
The user will be locked (not able to login) after reaching predefined maximum number of unsuccessful attempts to login. (Bear in mind that only camunda administartor is able to unlock users.)

This mechanism is not applicable if the system uses LDAP configuration. Each LDAP vendor offers similar mechanisms if you want to achieve the same behaviour. 

# Feature 1 

# Feature 2 

+++
author = "Seif Eddine Ghezala"
categories = ["Execution"]
tags = ["Release Note"]
date = "2018-02-02T12:00:00+01:00"
title = "Camunda 7.9.0-alpha1 Released"
+++

Camunda 7.9.0-alpha1 is here and it is packed with new features. The highlights are:

* Internationalization of Cockpit Full (enterprise)
* Conditional Start Event
* 
* 
* [32 Bug Fixes](https://app.camunda.com/jira/issues/?jql=issuetype%20%3D%20%22Bug%20Report%22%20AND%20fixVersion%20%3D%207.7.0-alpha1)

The [complete release notes](https://app.camunda.com/jira/secure/ReleaseNote.jspa?projectId=10230&version=14609) are available in Jira.

[List of known Issues.](https://app.camunda.com/jira/issues/?jql=affectedVersion%20%3D%207.7.0-alpha1)

You can [Download Camunda For Free](https://camunda.org/download/)
or [Run it with Docker](https://hub.docker.com/r/camunda/camunda-bpm-platform/).

<!--more-->

## Internationalization of Cockpit Full (enterprise)
Internationalization (i18n) basically means that in addition to the English language, further translations can be added in a simple way.

**Cockpit Basic** of the Community Platform 
[supports i18n since Camunda BPM 7.8](http://blog.camunda.org/post/2017/10/camunda-bpm-780-alpha5-released/#internationalization-i18n-for-cockpit-admin).
With this release we also added this feature to **Cockpit Full** of the Enterprise Platform.

By default, the Camunda BPM Platform is only shipped with the English language. 
To add more translations, it is necessary to create or add a separate translation file to each webapp.
The selection of the language is performed automatically, based on the language settings in the browser.

A collection of several translations can already be found as a community extension in a 
[central repository](https://github.com/camunda/camunda-webapp-translations).
We highly appreciate your contribution to improve the current translations or to add new ones.

> Please bear in mind that this feature is only available in the Enterprise Platform of Camunda BPM.
> To try it out anyway, please request a [Free Trial](https://camunda.com/download/enterprise/) or a
> [Quote](https://camunda.com/enterprise/).

## Conditional Start Event
The conditional event defines an event which is triggered if a given condition is evaluated to true. We are introducing support for Conditional Start Event, until now we have conditional start event only for Event Subprocess.
This allows you to start processes if some condition is fulfilled. Let's check the below picture.

{{< figure src="start-condition.png" >}}

The process will be started if the temperature is higher than 22Co. But how is it possible to do this?
In Java API you can trigger the evaluation of the deployed process's start conditions via RuntimeService:
```java
List<ProcessInstance> instances = runtimeService
    .createConditionEvaluation()
    .setVariable("temperature", 24)
    .evaluateStartConditions();
```
The REST API call would look like:
POST /condition
```json
{
  "variables" : {
    "temperature" : {"value" : 24, "type": "Integer",
                    "valueInfo" : { "transient" : true } },
    "city" : {"value" : "Parma", "type": "String"}
  },
  "businessKey" : "aBusinessKey",
  "tenantId" : "aTenantId"
}
```
For more details, please see the documentation about the [Java API](https://docs.camunda.org/manual/latest/reference/bpmn20/events/conditional-events/#conditional-start-event) and the
[REST API](http://docs.camunda.org/manual/latest/reference/rest/condition/post-condition/).

## FEATURE 3

## What's Next?
The next alpha version is scheduled for the end of February and our team is already working on it.

If you are curious about what the team is cooking for the next releases, here are few highlights:

* UPCOMING FEATURE 1
* UPCOMING FEATURE 2

You can also find out more details if you check out our [roadmap](https://camunda.org/roadmap).

## Your Feedback Matters!
Your feedback is extremely important for us in order to improve Camunda BPM, so your thoughts are always highly appreciated and considered by our team.

Feel free to share your ideas and suggestions with us by writing a post in the [forum](https://forum.camunda.org/).

Furthermore, if you have any feedback related to User Experience, things that keep bugging you, things that you think should work differently etc., please share your thoughts with us at [https://camundabpm.userecho.com](https://camundabpm.userecho.com)

+++
author = "Seif Eddine Ghezala"
categories = ["Execution"]
tags = ["Release Note"]
date = "2018-02-02T12:00:00+01:00"
title = "Camunda 7.9.0-alpha1 Released"
+++

Camunda 7.9.0-alpha1 is here and it is packed with new features. The highlights are:

* Internationalization of Cockpit Full (enterprise)
* Webapps: drill down in Metrics Charts and general facelift
* Core Engine: drastic reduction of optimistic locking exceptions when using Job Executor
* External Tasks: History API and monitoring in Cockpit
* Fluent BPMN builder API: now generates BPMN diagram elements
* Supported Environments: IBM Websphere 9 and OpenJDK 8
* Security: salts and stronger hashing of user passwords
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

## FEATURE 2

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

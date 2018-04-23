+++
author = "Fabian Hinsenkamp"
categories = ["Execution"]
tags = ["Release Note"]
date = "2018-04-27T15:00:00+01:00"
title = "Camunda 7.9.0-alpha4 Released"

+++

Camunda BPM platform 7.9.0-alpha4 is here and the highlights are:

* External task notification for long polling fetch and lock handler
* Feature 2
* Feature 3
* [XX Fixes](https://app.camunda.com/jira/issues/?jql=issuetype%20%3D%20%22Bug%20Report%22%20AND%20fixVersion%20%3D%207.9.0-alpha4)

You can [Download Camunda for free](https://camunda.com/download/) (click on Preview Release) or [Run it with Docker](https://hub.docker.com/r/camunda/camunda-bpm-platform/).


To see a full list of the changes, please check out our [Release Notes](https://app.camunda.com/jira/secure/ReleaseNote.jspa?projectId=10230&version=15301)
and the list of [Known Issues](https://app.camunda.com/jira/issues/?jql=affectedVersion%20%3D%207.9.0-alpha4).


If you want to dig in deeper, you can find the source code on [GitHub](https://github.com/camunda/camunda-bpm-platform/releases/tag/7.9.0-alpha3).

## External task notification for long polling fetch and lock handler

With the Camunda 7.9.0-alpha2 release, we introduced a long polling fetch and lock handler for External Tasks. This provided a reduced number of requests and a more efficient use of client, and server side resources.

However, it was possible that a new External Task had been created before the long polling timer ran out. And it would have been great if the clients were notified immediately about this change. Now, through the implementation of an ExternalTask notification for the long polling fetch and lock handler, this is possible. Immediately after a new ExternalTask is created, the handler is notified that a new External Task exists. Then, the  handler interupts the suspension periods and attempts to respond with the new External Task to some of the pending client requests.

## Feature 2

## Feature 3

## What's Next?

The next alpha version is scheduled for the end of May and our team is already working on it.

Here is a highlight if you want to know what the team is preparing for the next releases:

* Next Feature 1
* Next Feature 2


You can also find out more details if you check out our [roadmap](https://camunda.com/learn/community/#roadmap).



## Your Feedback Matters!

With every release we constantly strive to improve Camunda BPM. To make this possible, we are reliant on your feedback. Feel free to share your ideas and suggestions with us.

You can contact us by writing a post in the [forum](https://forum.camunda.org/).

Furthermore, if you have any feedback related to User Experience, things that keep bugging you, things that you think should work differently etc., please share your thoughts with us at [https://camundabpm.userecho.com](https://camundabpm.userecho.com)
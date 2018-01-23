+++
author = "Sebastian Stamm"
categories = ["Technical Analysis"]
date = "2018-01-18T16:30:00Z"
tags = ["Technical Analysis"]
title = "Camunda Tokenview - 3D Process Diagrams in Cockpit"

+++

Those of you who were at BPMCon 2017 will probably remember the interactive experience where you could walk around a BPMN process in virtual reality. This blog post explores how such a project can be brought into Camunda Cockpit using the [Plugin mechanism](https://docs.camunda.org/manual/latest/webapps/cockpit/extend/plugins/). We will explore how to

 - Add new UI elements to Cockpit views
 - include new third party libraries
 - Use the REST-API to communicate with the Backend


<!--
In the context of the Camunda Hackdays (Video to the Hackdays), we built an interactive experience where you could explore your processes in virtual reality. After presenting the project at BPMCon 2017 I was looking for a way to make this experience available for a wider audience. That was the birth of Camunda Tokenview (Link to Github Repository). What is Camunda TokenView:
 - Cockpit Plugin
 - Allows you to "jump into" your processes
 - see real Tokens running through your process
-->

<!--more-->

# Getting Started

Before we begin: "Be a Token" and "Camunda Tokenview" are personal projects I do beside my regular work as Core Developer at Camunda. Therefore, the code and algorithms are merely "prototype-quality" and should not be used in production or any serious environments.

Now that we have that out of the way, I invite you to head over to [my Github Repository](https://github.com/SebastianStamm/camunda-tokenview) to download the project and follow the installation instructions for your Camunda instance. This blog post assumes you use a fresh copy of the [Camunda Platform](https://camunda.com/download/).

<!-- Code Quality is usually a primary concern at Camunda. These Hackday projects however are more for a quick exploration of ideas and build for implementing stuff fast. The code certainly has bugs, uses global variables and many anti-patterns for the sake of Getting Things Done. -->

# Let's Begin

The [Camunda documentation](https://docs.camunda.org/manual/latest/webapps/cockpit/extend/plugins/#the-nature-of-a-cockpit-plugin) describes a Cockpit plugin as "a maven JAR project that is included in the cockpit webapplication as a library dependency". While this approach would give us most flexibility on the front- and backend, we are focusing on the frontend part and are fine with using the [REST-API](https://docs.camunda.org/manual/latest/reference/rest/) to communicate with the backend. In this scenario, we can use [frontend-only plugins](https://github.com/camunda/camunda-bpm-examples/tree/master/cockpit/js-only-plugin), which are much easier to setup.

When you start developing a plugin, one of the first questions you need to answer is what part of which page you want to augment. In our case, we want to add some content to the [Process Instance Diagram](https://docs.camunda.org/manual/latest/webapps/cockpit/extend/plugins/#process-instance-diagram-overlay).



# The Entry Point

 - Plugin without Backend
 - Example Code for a frontend only plugin at [Camunda Examples Repo]
 - Checking available Plugin Points at [Camunda Docs]
 - Adding new content
   - Some trivialised sample code
   - Link to the index.js file in the github repo (mention that it also includes some basic drag and drop mechanics)


# Loading Libraries

 - usually libraries are loaded via custom scripts --> more setup and stuff, do not want that
 - fix: dynamically add a script tag to the header pointing to aframe CDN
 - while we are at it, add also other Script tags registering the aframe components
 - advantage: no need for some build system compiling all our javascript in one file AND simple setup
 - disadvantage: polluting global namespace

# Displaying Tokens

 - Moving around and rendering the process is copied mostly one to one from the "Be a token" project
 - Something that we can do that "Be a Token" cannot do is: "Displaying tokens"
 - Getting the information from activity-instance history endpoint (basically the audit log)
 - using this information to reconstruct the life of a single token (and figuring out where it currently is --> last stage)

# Animating Tokens

 - Constantly polling the endpoint and check if something changed
 - If yes, we do the same algorithm and animate the tokens

# Deleting Tokens

 - Something I get asked all the time (Can I shoot the tokens)
 - gun is from google poly: https://poly.google.com/view/aLbeOSoOpXN
 - adding a gun, some raycasting, and a REST call to cancel the token (same one that the process instance modification feature uses)

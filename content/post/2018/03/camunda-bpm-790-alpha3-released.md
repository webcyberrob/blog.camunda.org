+++
author = "Nikola Koevski"
categories = ["Execution"]
tags = ["Release Note"]
date = "2018-03-29T16:00:00+01:00"
title = "Camunda 7.9.0-alpha3 Released"

+++

Announcing the third alpha version of Camunda BPM 7.9.0 and there are some pretty new features. The highlights are:

* Payload in Signal Events
* Feature 2
* Feature 3
<!-- TODO(Nikola) Add NUM Fixes -->
* [NUM Fixes]()

<!-- TODO please add your beautiful explanations to the new features below -->

## Payload in Signal Events

This alpha release adds support for payload in Signal Events.

Signal Events are broadcast to all of the signal-catching Process Instances. However, at times, additional information (Process Variables, Business Keys, etc.) is needed for the Signal Event to be properly utilized. Previously, this was only possible by using workarounds which could result in a semantically incorrect BPMN diagram. Furthermore, these workarounds had to be implemented with custom code which added to the complexity of the solution.

{{< figure class="teaser no-border" src="signal_no_payload.png" alt="Signal Events with no payload" >}}

From now on, a Signal Payload can be used out-of-the-box with the Camunda BPM Platform. It is possible to pass to the Signal-catching instances:

* All of the Signal-throwing Process Instance Variables;
* Only the local execution variables;
* Single Process Variables along with their target names;
* Expressions, to further manipulate the value of the target variables;
* A Business Key, for Signal-catching Process Instances starting with a Start Signal Event.

{{< figure class="teaser no-border" src="signal_with_payload.png" alt="Payload feature for Signal Events" >}}

The Signal payload is declared in the Throwing Signal Event Definition, meaning that the same Signal can be thrown with different payloads at different points of the diagram.

Finally, the Signal Definition with payload can be created using XML:

```xml
<signalEventDefinition signalRef="thePizzaSignal">
    <extensionElements>
        <camunda:in source="X" target="Y" />
        <camunda:in sourceExpression="${X + 5}" target="Z" />
        <camunda:in variables="all" local="true" />
        <camunda:in businessKey="${execution.processBusinessKey}" />
    </extensionElements>
</signalEventDefinition>
```

Or, by using the (Java) Fluent Builder API:

```java
BpmnModelInstance modelInstance = Bpmn.createProcess()
  .startEvent()
  .intermediateThrowEvent("thePizzaSignal")
    .signalEventDefinition("signal")
      .camundaInSourceTarget("X", "Y")
      .camundaInSourceExpressionTarget("${X + 5}", "Z")
      .camundaInAllVariables("all", true)
      .camundaInBusinessKey("${execution.processBusinessKey}")
      .throwEventDefinitionDone()
  .endEvent()
  .done();
```


## Feature 2

<!-- Feature 2 text here -->

## Feature 3

<!-- Feature 3 text here -->

<!-- the features' explanations end here -->

## What's Next?

The next alpha version is scheduled for the end of April and our team is already working on it.

Here are a few highlights if you want to know what the team is preparing for the next releases:

<!-- TODO(Nikola) Add next highlight -->
* Next Highlight

You can also find out more details if you check out our [roadmap](https://camunda.com/learn/community/#roadmap).

## Your Feedback Matters!

Your feedback is extremely important for us in order to improve Camunda BPM, so your thoughts are always highly appreciated and considered by our team.

Feel free to share your ideas and suggestions with us by writing a post in the [forum](https://forum.camunda.org/).

Furthermore, if you have any feedback related to User Experience, things that keep bugging you, things that you think should work differently etc., please share your thoughts with us at [https://camundabpm.userecho.com](https://camundabpm.userecho.com)

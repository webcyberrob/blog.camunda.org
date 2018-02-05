Those of you who were at BPMCon 2017 will probably remember the interactive experience "Be A Token" where you could walk around in BPMN process in virtual reality. This blog post explores how we built the 3D rendering of bpmn processes using [bpmn-js](https://github.com/bpmn-io/bpmn-js) and [A-Frame](https://aframe.io/).

<!-- more -->

# Getting a Javascript representation of the process

A [BPMN file](https://github.com/bpmn-io/bpmn-js/blob/master/test/fixtures/bpmn/basic.bpmn) is an XML file containing information about all the elements, their properties and how they should be displayed. Javascript is not very good at dealing with XML directly, so before we can do anything useful with the file, we need to convert it into a format we can work with.

Fortunately, the [bpmn-js](https://github.com/bpmn-io/bpmn-js) library does that for us. Usually, the library is used to render a bpmn process. We are going to use it's methods to access the underlying logical structure of the process to create our own (3D-)rendering. We start with importing the model like described on the bpmn-js Github page:

    const BpmnViewer = require('bpmn-js');

    const xml; // my BPMN 2.0 xml
    const viewer = new BpmnViewer({ container: document.createElement('div') });

    viewer.importXML(xml, function() {
      // our rendering takes place here
    });

Now we need to access the elements and their specified position in order to create a 3D representation of them:

    const elementRegistry = viewer.get('elementRegistry');

    elementRegistry.forEach(element => {
      if(element.businessObject.$instanceOf('bpmn:Task')) {
        renderTask(element);
      } else if(element.businessObject.$instanceOf('bpmn:Gateway')) {
        renderGateway(element);
      }
      //...
    })

The elements themselves have x, y, width and height properties which gives us all the information we need to create a 3D representation.

# About 3D rendering

A three-dimensional model in the context of computer graphics consists of vertices and faces. A vertex defines a position in space and has three coordinates: x, y and z. A face is a triangle, connecting three vertices to a surface.

For example, a cube would consist of 8 vertices (one for every corner) and 12 faces (6 sides and each side can be represented by 2 triangles)*

_*In 3D modeling, a face sometimes consists of 4 or more vertices. In our scenario, we assume that every face has exactly three vertices_

The next sections describe the construction of the 3D model for different BPMN elements.

# Sequence Flows

In "Be a Token" sequence flows are represented a hallway. In the bpmn file, a sequence flow element has a waypoints array, which contains all points where the sequence flow line changes direction plus the start- and endpoint of the sequenceflow. A graphical representation would look like this:

!Image here!

As you can see, a sequence flow is a single line and does not specify a width. When rendering a sequence flow in 3D, in order to move "through" the sequence flow, it needs to have some width to be represented as a hallway. To achieve that, we split the sequence flow into multiple straight-line segments. Then, we create vertices that are perpendicular to the sequence flow segment. These newly created vertices can be connected as two faces to form the floor of this sequence flow segment hallway:

!Image here!

Whenever we deal with a sequence flow that is not a straight line, we create multiple of those hallway segments that need to be merged. At the waypoint where the sequence flow changes direction, the two vertices that define the hallway do not align. To fix that, we create a new vertex which is between the two original vertices and has the same distance to the hallway center. This creates a closed continuous hallway for the sequence flow. The following images illustrate this process:

!3 Images here!

This algorithm creates the vertices for the floor of the sequence flow. To create walls and a ceiling, every vertex is copied and the z-Position of each copy is set to the desired height of the sequence flow hallway.

Now that we have a 3D representation of sequence flows, let's look at tasks:

# Tasks

In "Be a Token", tasks are represented by rooms sequence flows connect to. A basic room would just be a cube the player can stand inside of. However, since we want to connect sequence flow hallways to this room, we need to leave some space in the walls which leads to a more interesting configuration of vertices and faces. This is a single wall with the position of its vertices and faces of a task in "Be A Token":

!Image!

The floor and ceiling of the task room can just re-use the vertices created by the four walls.

Note that the placement of the "hole" in the wall for the sequence flow is always in the same place. That means that the connection to a sequence flow also always has to be in the same place. This works well with the manhattan-layout, where only 90° angles are allowed. A sequence flow connecting to the corner of a task will not be rendered correctly.

When a wall


# Resources

You can find the sourcecode of "Be A Token" [here](https://github.com/SebastianStamm/BAT).

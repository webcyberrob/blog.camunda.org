Those of you who were at BPMCon 2017 will probably remember the interactive experience "Be A Token" where you could walk around in BPMN process in virtual reality. This blog post explores how we built the 3D rendering of bpmn processes using [bpmn-js](https://github.com/bpmn-io/bpmn-js) and [A-Frame](https://aframe.io/).

<!-- Add a link to the game where they can play it instantly -->

<!-- Add a link to the game where they can upload their own process diagrams -->

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

Note that the placement of the "hole" in the wall for the sequence flow is always in the same place. That means that the connection to a sequence flow also always has to be in the same place. This works well with the manhattan-layout, where only 90° angles are allowed. A sequence flow connecting to the corner of a task will not be rendered correctly. For "Be a Token" this is not a problem as the levels can be designed accordingly.

When the geometry of the wall is generated like described above, the hole in the wall will always be there, even when there is no sequence flow attached. In "Be a Token" this is fixed the same way you might fix a hole in the wall in your home: By hiding it with a poster. When there is no sequence flow attached, we render a image in the color of the wall in that position, so that the player cannot see through this hole. If you are curious how this is done, you can find the code [here](https://github.com/SebastianStamm/BAT/blob/1f4a480c924bc7a236d2a629c500a809c783f9a8/task.js#L378).

!Image of doors!

<!-- door mechanism with link to source code -->
We can use the same mechanism to implement a door mechanism: When we detect that a sequence flow is attached to a side of a task, a semi-transparent image is created and placed on top of that opening. Since a pure white image looks a bit off, I put the Camunda logo on there to make it look like it is a trendy glass-door. Since I did not want to deal with collission detection for doors, I decided to make them open automatically when the player is near them. Since the player is already in an unusual and futuristic scenario as a token in a BPMN process, I decided to make to doors slide open vertically, like in a spaceship.

This looks like the door disappears in the ceiling, but actually it goes through the ceiling. If the player uses cheats to get out of bounds of the process like shown in the picture below, he can see that the door is actually still there. The implementation of the door opening and closing is done in the [collission component](https://github.com/SebastianStamm/BAT/blob/1f4a480c924bc7a236d2a629c500a809c783f9a8/collision.js).

!GIF of door opening and closing + getting out of bounds!


# Gateways and Events

<!-- Motivation for handling gateways and events the same way - putting a texture on the ground indicating what it is -->

<!-- Description of the vertices and faces -->


# Resources

You can find the sourcecode of "Be A Token" [here](https://github.com/SebastianStamm/BAT).

<!-- add ready to play version online somewhere -->

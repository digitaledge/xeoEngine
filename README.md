SceneJS-Nexus
=============

SceneJS Nexus is an insanely modular 3D engine built on [ActorJS](http://actorjs.org) and [SceneJS](http://scenejs.org).

## Concept

Via JSON-RPC calls, Nexus lets us plug together a world from actor components, which we then call methods on
to make them do stuff.

JSON-RPC is very sweet for controlling 3D worlds. It lets us do things like distribute them across networks, drive them
with scripts written in other languages and so on.

```javascript

/* Get an ActorJS instance to manage our 3D world
 */
require([
    '../js/actorjs'
    ],
    function (world) { // We'll call the instance "world"

        /* Tell our ActorJS instance where to find the AMD modules
         * that define our actor types
         */
        world.configure({
            actorClassPath:"actors/"
        });

        /* Add a root actor that provides a SceneJS scene graph to its child
         * actors, complete with lookat node and lights.
         */
        world.call("addActor", {
            type:"scene",
            actorId:"scene",
            canvasId:"theCanvas"
        });

        /* Add a child teapot actor to the root actor. This will create
         * a teapot in the scene graph.
         */
        world.call("scene/addActor", {
            type:"objects/prims/teapot",
            yPos:2
        });

        /* Add a child actor to the root actor. This will control the
         * scene graph's lookat node.
         */
        world.call("myScene/addActor", {
            type: "scene/camera",
            actorId: "myCamera"
        });

        /* Subscribe to the "eye" topic that is published to by
         * the camera actor whenever its eye position changes.
         * See how we specify a path down through the hierarchy
         * to the camera actor's topic.
         */
        world.subscribe("myScene/myCamera/eye",
            function(update) {

                var eye = update.eye;

                //..
            });

        /* Call a method on the camera actor to set the eye position.
         * This will cause the actor to publish to its "eye" topic,
         * which will get handled by the subscription we made above.
         */
         world.call("myScene/myCamera/setEye", {
            x: -30,
            y: 0,
            z: 50
        });

    });

```

## Documentation
To get the gist of Nexus, take a look at
* [ActorJS](http://actorjs.org)
* [SceneJS](http://scenejs.org)
* [The Nexus Wiki](https://github.com/xeolabs/scenejs-nexus/wiki)

## Examples
[Teapot](http://xeolabs.github.com/scenejs-nexus/teapot.html) - A Newell Teapot resting on a grid ground plane
[City](http://xeolabs.github.com/scenejs-nexus/city.html) - A procedurally-generated city
[City Flight](http://xeolabs.github.com/scenejs-nexus/cityFlight.html) - Flight simulation over a procedurally-generated city

## License
SceneJS Nexus is licensed under both the [GPL](https://github.com/xeolabs/scenejs-nexus/blob/master/licenses/GPL_LICENSE.txt)
and [MIT](https://github.com/xeolabs/scenejs-nexus/blob/master/licenses/MIT_LICENSE.txt) licenses. Pick whichever of those fits your needs.

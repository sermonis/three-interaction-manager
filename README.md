# THREE.Interactive

[![NPM Package](https://img.shields.io/npm/v/three.interactive.svg?style=flat)](https://www.npmjs.com/package/three.interactive)

Fast and simple interaction manager for [THREE.js](https://github.com/mrdoob/three.js/) for enabling mouse and touch events on 3D objects.

*Note: When using ReactJS I can highly recommend [react-three-fiber](https://github.com/pmndrs/react-three-fiber), which has built-in interaction support. For pure THREE.js projects, this little library can be very useful though.*

### Hot it works:

Intersections are sorted by distance to the camera and the events are dispatched in that order (closest first). If InteractiveEvent.stopPropagation() is called, the event won't fire again on other objects.

Alternative to [three.interaction](https://github.com/jasonChen1982/three.interaction.js).

Collaborations and improvements are welcome.


### Examples ####

* [Simple](https://dev.markuslerner.com/three.interactive/examples/simple.html): Basic example
* [Depth](https://dev.markuslerner.com/three.interactive/examples/depth.html): Overlapping objects example
* [glTF](https://dev.markuslerner.com/three.interactive/examples/gltf.html): Hover/click gltf objects example

### Usage ####

1. Include script:

```
import { InteractionManager } from "three.interactive";
```

2. Create an InteractionManager instance

```
const interactionManager = new InteractionManager(
  renderer,
  camera,
  renderer.domElement
);
```

3. Add object to InteractionManager

```
interactionManager.add(cube);
```

4. Add event listener to object

```
cube.addEventListener("click", (event) => {
});
```

5. Call InteractionManager.update() on each render

```
interactionManager.update();
```


### Simple example ####

```
import * as THREE from "three";
import { InteractionManager } from "three.interactive";

const container = document.createElement("div");
container.setAttribute("id", "container");
document.body.appendChild(container);

const renderer = new THREE.WebGLRenderer();
renderer.setPixelRatio(window.devicePixelRatio);
renderer.setSize(window.innerWidth, window.innerHeight);
container.appendChild(renderer.domElement);

const scene = new THREE.Scene();

const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.set(0.0, 0.0, 10.0);

const interactionManager = new InteractionManager(
  renderer,
  camera,
  renderer.domElement
);

const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshBasicMaterial();

const cube = new THREE.Mesh(geometry, material);
cube.addEventListener("mouseover", (event) => {
  event.target.material.color.set(0xff0000);
  document.body.style.cursor = "pointer";
});
cube.addEventListener("mouseout", (event) => {
  event.target.material.color.set(0xffffff);
  document.body.style.cursor = "default";
});
cube.addEventListener("mousedown", (event) => {
  event.target.scale.set(1.1, 1.1, 1.1);
});
cube.addEventListener("click", (event) => {
  event.target.scale.set(1.0, 1.0, 1.0);
});
scene.add(cube);
interactionManager.add(cube);

const animate = (time) => {
  requestAnimationFrame(animate);

  interactionManager.update();

  renderer.render(scene, camera);
};

animate();
```

### API ###

#### InteractionManager class ####


```new InteractionManager(renderer, camera, renderer.domElement)``` – constructor InteractionManager instance

**Members:**

```treatTouchEventsAsMouseEvents``` (boolean) – wether touch events should fire as mouse events, default: true


**Methods:**

```interactionManager.add(object, childNames = [])``` – add object(s), optionally select only child objects by names

```interactionManager.remove(object, childNames = [])``` – remove object(s)

```interactionManager.update()``` – update InteractionManager on each render

```interactionManager.dispose()``` – dispose InteractionManager


#### InteractiveEvent class ####


**Members:**

```cancelBubble``` (boolean) – wether events should continue to bubble, default: false

```coords``` (THREE.Vector2) – Mouse/touch coords

```distance``` (number) – Distance of intersected point from camera

```intersected``` (boolean) – Whether object is still intersected

```originalEvent``` (Event object) – Original event, if available (mouse, touch)

```target``` (THREE.Object3D) – Target object

```type``` (string) – event type: 'click', 'mousedown', 'mousemove', 'mouseup', 'touchstart', 'touchmove', 'touchend'


**Methods:**

```stopPropagation``` – stop bubbling of event (cancelBubble), e.g. when only the object closest to the camera is supposed to fire an event


#### License ####

MIT licensed

Copyright (C) 2020 Markus Lerner, http://www.markuslerner.com

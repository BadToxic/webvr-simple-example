# Web VR BadToxic example

With this I want to show you, how you can easily create your own VR experience using _WebVR Boilerplate_:

 A _[THREE.js](http://threejs.org "THREE.js")_-based starting point for VR experiences that work well in both Google Cardboard and other VR headsets. Also provides a fallback for experiencing the same content without requiring a VR device.
 
The result will look like this in the end:

![Screenshot of the result in Chrome](https://raw.githubusercontent.com/BadToxic/webvr-simple-example/master/screenshots/webvr-toxicon-example-chrome.png "Screenshot of the result in Chrome")

This screenshot was taken in Chrome (_Version 50.0.2661.102 m_) without the VR mode enabled.
You can check it out yourself by visiting _[http://badtoxic.de/webvr](http://badtoxic.de/webvr "Web VR BadToxic example")_.
If your browser is compatible to the VR mode you will see a little cardboard button on the bottom right. I testet the VR mode with Android, iOS (Safari on iPhone 6+, iOS 9.3.2) and a [special chromium build](https://webvr.info/get-chrome/ "special chromium build") on my pc with a HTC Vive. You also can use [Firefox Nightly with an Oculus Rift enabler installed](https://mozvr.com/#start "Firefox Nightly").
For more info visit [webvr.info](https://webvr.info/ "webvr.info").

## Getting started

- At first you should clone or download [Boris’ WebVR Boilerplate repo](https://github.com/borismus/webvr-boilerplate "Boris’ WebVR Boilerplate repo")

`git clone https://github.com/borismus/webvr-boilerplate.git`

- Then you can upload this to your web server or start a local one, similar to way you’d have to spin up a server for any regular web app like
 
`cd webvr-boilerplate/`

`python -m SimpleHTTPServer 8000`

To test that your server is running, open up localhost on the port that your server is talking to. (Default: http://localhost:8000)

Alternatively you can use a Dropbox `Public` folder, if you have one, like I did here:
[Web VR BadToxic example on Dropbox](https://dl.dropboxusercontent.com/u/42226641/webvr-simple-example/index.html "Web VR BadToxic example on Dropbox")

You should now see the default boilerplate scene, with a spinning cube against a black background with green gridlines.

 ![Default scene](https://raw.githubusercontent.com/BadToxic/webvr-simple-example/master/screenshots/webvr-boilerplate-defailt-scene.png "Default scene")

Congratulations! You just deployed your first VR web app.

- Now let us start modifying the scene by replacing the background grid with a skybox sphere.
So first remove everything that has to do with loading the background texture `img/box.png` and the `skybox`. Instead we load another texture we placed in the img folder - for example a skybox.png we may have found via google by searching "skybox"... But remember that textures always have to have any power of 2 size like 1024x512. So I just found this image:

![Sky](https://raw.githubusercontent.com/BadToxic/webvr-simple-example/master/img/skybox.png "Sky")
Source: [http://voodoomagic-solinvictus.blogspot.de/2011/03/sky-box-template-texture.html](http://voodoomagic-solinvictus.blogspot.de/2011/03/sky-box-template-texture.html "Sky source")

And so our code may look like the following:
```javascript
var textureLoader = new THREE.TextureLoader();
var skybox;
textureLoader.load('img/skybox.png', function (skyboxTexture) {
  var skyboxMaterial = new THREE.MeshBasicMaterial();
  skyboxMaterial.map = skyboxTexture;
  skyboxMaterial.side = THREE.BackSide;

  // Create the mesh based on geometry and material
  var skyboxSphereGeometry = new THREE.SphereGeometry(50, 200, 200);
  var skyboxmesh = new THREE.Mesh(skyboxSphereGeometry, skyboxMaterial);
  skybox = new THREE.Mesh(skyboxSphereGeometry, skyboxMaterial);
  scene.add(skybox);
};
```
Here we crated a texture loader and call its function `load` for our new image. The function also need a further function that will be called when loading the texture has finished. When this asynchronous function is called, it will create the skybox material with the loaded texture and instanciate the skybox we then add to our scene.

- After that we may want to add some fancy texture to our cube. So we just do load another texture like in the previous step and bind it to the cube.
So I added the following code just after the `scene.add(skybox);` of the last one (inside our asynchronous function of the skybox).
```javascript
textureLoader.load('img/toxicon_logo.png', function (toxiconLogoTexture) {
  var toxiconCubeGeometry = new THREE.BoxGeometry(0.5, 0.5, 0.5);
  var toxiconCubeMaterial = new THREE.MeshBasicMaterial();
  toxiconCubeMaterial.map = toxiconLogoTexture;
  toxiconCube = new THREE.Mesh(toxiconCubeGeometry, toxiconCubeMaterial);

  // Position cube mesh to be right in front of you.
  toxiconCube.position.set(0, controls.userHeight, -1);
  
  // Add cube mesh to your three.js scene
  scene.add(toxiconCube);
  
  // For high end VR devices like Vive and Oculus, take into account the stage
  // parameters provided.
  setupStage();
});
```


## Finished!

So - this may be enough for our little tutorial. Our code now results in the following scene.

![Example in chromium](https://raw.githubusercontent.com/BadToxic/webvr-simple-example/master/screenshots/webvr-toxicon-example-chromium.png "Example in chromium")

This time I used the mentioned chromium build and startet the VR mode. That's why we see two views - one for each eye.
If you have any questions or suggestions, feel free to use the comment function below.

Thank you for reading and see you later!
BTW, also my Mii likes VR!

![my Mii likes VR](https://raw.githubusercontent.com/BadToxic/webvr-simple-example/master/screenshots/BadToxic%20Mii%20Vive.png "my Mii likes VR")




Sources:

[WebVR](https://webvr.info/ "WebVR")

[WebVR Boilerplate](https://github.com/borismus/webvr-boilerplate "WebVR Boilerplate")

[Responsive WebVR](http://smus.com/responsive-vr "Responsive WebVR")

[4 Steps to Start Experimenting with WebVR in 10 Minutes](http://www.roadtovr.com/4-steps-to-start-experimenting-with-webvr-in-10-minutes/ "4 Steps to Start Experimenting with WebVR in 10 Minutes")


See also:

[MOZVR](https://mozvr.com/ "MOZVR")

[WebVR Samples](https://toji.github.io/webvr-samples/ "WebVR Samples")

# Web VR BadToxic Beispiel

Heute möchte ich euch zeigen, wie man ganz einfach eine eigene VR-Erfahrung mit dem _WebVR Boilerplate_ erstellt:

 Ein _[THREE.js](http://threejs.org "THREE.js")_-basierter Ausgangspunkt für VR Erfahrungen, welcher sowohl Google-Cardboards als auch andere VR-Headsets unterstützt. Außerdem wird ein Fallback angeboten, der das Erleben des gleichen Inhalts erlaubt, ohne dass eine VR-Vorrichtung erforderlich ist.
 
Das Ergebnis am Ende wird wie folgt aussehen:

![Screenshot of the result in Chrome](https://raw.githubusercontent.com/BadToxic/webvr-simple-example/master/screenshots/webvr-toxicon-example-chrome.png "Screenshot of the result in Chrome")

Dieser Screenshot wurde in Chrome (_Version 50.0.2661.102 m_) ohne den VR-Modus gemacht.
Ihr könnt es selbst auf _[http://badtoxic.de/webvr](http://badtoxic.de/webvr "Web VR BadToxic Beispiel")_ begutachten.
Wenn euer Browser mit dem VR-Modus kompatibel ist, seht ihr einen kleinen Cardboard-Button in der rechten unteren Ecke. Ich habe den VR-Modus mit Android, iOS (Safari auf dem iPhone 6+, iOS 9.3.2) und einem [speziellem Chromium-Build](https://webvr.info/get-chrome/ "speziellem Chromium-Build") auf meinem PC mit einer HTC Vive getestet. Ihr könnt auch eine [Firefox Nightly Version mit dem Oculus Rift Enabler](https://mozvr.com/#start "Firefox Nightly") verwenden.
Für weitere Infos könnt ihr [webvr.info](https://webvr.info/ "webvr.info") besuchen.

## Los geht's

- Zuerst solltet ihr [Boris’ WebVR Boilerplate repo](https://github.com/borismus/webvr-boilerplate "Boris’ WebVR Boilerplate repo") klonen oder herunterladen

`git clone https://github.com/borismus/webvr-boilerplate.git`

- Dies könnt ihr dann auf euren Webserver hochladen oder einen lokalen starten, genauso, wie ihr es für jede andere Web-Anwendung tun würdet
 
`cd webvr-boilerplate/`

`python -m SimpleHTTPServer 8000`

Um zu testen, ob der Server läuft, öffnet localhost auf dem Port, mit dem der Server spricht. (Standard: http://localhost:8000)

Alternativ könnt ihr einen Dropbox `Public`-Ordner verwenden, wenn ihr einen habt, wie ich es hier gemacht habe:
[Web VR BadToxic Beispiel auf Dropbox](https://dl.dropboxusercontent.com/u/42226641/webvr-simple-example/index.html "Web VR BadToxic Beispiel auf Dropbox")

Ihr solltet nun die Standard-Boilerplate-Szene, mit einem sich drehenden Würfel vor einem schwarzen Hintergrund mit grünen Rasterlinien sehen.

 ![Default scene](https://raw.githubusercontent.com/BadToxic/webvr-simple-example/master/screenshots/webvr-boilerplate-defailt-scene.png "Default scene")

Glückwunsch! Ihr habt eure erste VR-Web-App erstellt.

- Nun lasst uns beginnen, indem wir die Szene modifizieren - wir ersetzen das Hintergrund-Gitter mit einer Skybox-Kugel.
Als erstes lasst uns alles entfernen, was mit dem Laden der Hintergrundtextur `img/box.png` und der alten `skybox` zu tun hat. Stattdessen laden wir eine andere Textur, die wir im `img`-Ordner abgelegt haben - zum Beispiel ein `skybox.png`, welches wir vielleicht durch eine Suche nach "skybox" mit Google gefunden haben... Aber denkt stets daran, dass Texturen immer Größen von 2er-Potenzen haben müssen, wie etwa 1024x512. Ich habe dieses Bild gefunden:

![Sky](https://raw.githubusercontent.com/BadToxic/webvr-simple-example/master/img/skybox.png "Sky")
Quelle: [http://voodoomagic-solinvictus.blogspot.de/2011/03/sky-box-template-texture.html](http://voodoomagic-solinvictus.blogspot.de/2011/03/sky-box-template-texture.html "Sky source")

Und so kann unser Code wie folgt aussehen:
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
Hier haben wir einen Textur-Loader erstellt und rufen seine Funktion `load` auf, um unser neues Bild zu laden. Die Funktion benötigt auch eine weitere Funktion, die aufgerufen wird, wenn das Laden der Textur abgeschlossen wurde. Wenn diese asynchrone Funktion aufgerufen wird, wird das Skybox-Material mit der geladenen Textur erstellen und die Skybox instanziiert, welche wir dann zu unserer Szene hinzufügen.

- Danach können wir unserem Würfel eine ausgefallene Textur hinzufügen. Also laden wir einfach eine andere Textur wie im vorherigen Schritt und binden diese an den Würfel.
So habe ich den folgenden Code direkt nach dem `scene.add(skybox);` des letzten Code-Ausschnitts hinzugefügt (in unserer asynchronen Funktion der Skybox).
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


## Fertig!

Also - das sollte für unser kleines Tutorial ausreichen. Unser Code resultiert nun in folgende Szene.

![Example in chromium](https://raw.githubusercontent.com/BadToxic/webvr-simple-example/master/screenshots/webvr-toxicon-example-chromium.png "Example in chromium")

Dieses Mal habe ich den erwähnten Chromium-Build verwendet und den VR-Modus gestartet. Deshalb sehen wir zwei Bilder - eines für jedes Auge.
Wenn ihr Fragen oder Anregungen habt, fühlt euch frei unten die Kommentar-Funktion zu nutzen.

Vielen Dank für's Lesen - man sieht sich!
Übrigens, auch mein Mii mag VR!

![my Mii likes VR](https://raw.githubusercontent.com/BadToxic/webvr-simple-example/master/screenshots/BadToxic%20Mii%20Vive.png "my Mii likes VR")




Quellen:

[WebVR](https://webvr.info/ "WebVR")

[WebVR Boilerplate](https://github.com/borismus/webvr-boilerplate "WebVR Boilerplate")

[Responsive WebVR](http://smus.com/responsive-vr "Responsive WebVR")

[4 Steps to Start Experimenting with WebVR in 10 Minutes](http://www.roadtovr.com/4-steps-to-start-experimenting-with-webvr-in-10-minutes/ "4 Steps to Start Experimenting with WebVR in 10 Minutes")


Siehe auch:

[MOZVR](https://mozvr.com/ "MOZVR")

[WebVR Samples](https://toji.github.io/webvr-samples/ "WebVR Samples")

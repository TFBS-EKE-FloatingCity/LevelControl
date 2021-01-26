#Allgemein

Das 3D-Modell wird mit einer JavaScript Library eingebunden. Die verwendete Library heißt „three.js“ und man findet auf ihrer Homepage (https://threejs.org) genügend Dokumentationen zu den einzelnen Features.
Mit der Library wird das Modell über einen Pfad geladen, anschließend positioniert und je nach Sensor-Daten auf der x, y und z Achse verschoben.


#Laden des 3D-Modells

Im Skript „3d-model.js“ findet man einen Code Abschnitt in dem das 3D-Modell geladen wird. Dieser Abschnitt funktioniert etwa so:

    const loader = new GLTFLoader();

    loader.load( 'path/to/model.glb', function ( gltf ) {

	    scene.add( gltf.scene );

    }, undefined, function ( error ) {

	    console.error( error );

    } );

In einem String gibt man den Dateipfad an, der zum 3D-Modell führt. Falls das Laden nicht funktioniert wird ein Fehler in der Konsole ausgegeben. Wenn das Modell erfolgreich geladen wurde, kann man es anschließend der Scene hinzufügen, also auf der virtuellen Umgebung erscheinen lassen.

Weitere Informationen findet man hier:
https://threejs.org/docs/index.html#manual/en/introduction/Loading-3D-models

##Größe & Position des 3D-Modells

Um die Größe und Position des Modells anzupassen kann man folgendes machen:  

**Größe:**
- gltf.scene.scale.x = WERT ( = in Radianten);
- gltf.scene.scale.y = WERT;
- gltf.scene.scale.z = WERT;

**Position:**
- gltf.scene.position.x = WERT;
- gltf.scene.position.y = WERT;
- gltf.scene.position.z = WERT;

Das wird im Skript „3d-model.js“ z.B. innerhalb der Funktion, die nach erfolgreichem Laden ausgeführt wird gemacht. Dort wird das Objekt für den Start mittig positioniert. 
 
##Neigung des 3D-Modells
Die Neigung auf den Achsen kann über folgende Werte angegeben werden:

**Neigung:**
- cube.rotation.x = WERT;
- cube.rotation.y = WERT;
- cube.rotation.z = WERT;

Das wird im Skript „3d-model.js“ innerhalb der render() - Funktion gemacht. Die render() - Funktion wird immer wieder von neu aufgerufen und deshalb werden so stets die neuen Werte die in der Funktion angegeben werden, gerendert.

Die Neigung wird leichter Verständlich, wenn man bedenkt, dass sich alles um die 3 Achsen dreht. Also, wenn die Neigung der Y-Achse sich stets erhöht dreht sich das Modell im Kreis.
Wenn die Neigung der X-Achse sich verändert wird das Modell nach vorne oder nach hinten gekippt und bei der Y-Achse passiert dasselbe aber seitlich.

##Höhe des 3D-Modells
Die Höhe des 3D-Modells kann nur über eine Funktion zur bestehenden Höhe erweitert oder verringert werden. Das ist der Grund warum man die momentane Höhe am besten seperat in einer Variable speichert und somit einen Referenzwert hat.

**Höhe:**
- cube.translateY(WERT)

In der Anwendung würde man so die Höhe anpassen, falls die gewünschte Höhe bekannt wäre:
- cube.translateY(GEWÜNSCHTE-HÖHE - MOMENTANE-HÖHE)

#Größe der Scene anpassen

Momentan ist die Scene die ganze Weite des „#CityRotationChartPanel“ - DOMElements Hoch und Breit. Falls dies geändert werden will, können die Variablen height und width im Skript 
„3d-model.js“ geändert werden. Sie befinden sich am Anfang des Skripts.


#Partial View

Die Partial View findet man im „Views > Charts“ - Ordner unter „CityRotationChart.cshtml“.
Dort wird ein Titel und das Canvas, welches die virtuelle Umgebung enthalten wird gesetzt.
Des weiteren werden dann die notwendigen Skripts geladen: 

- jQuery
- three.js
- orbitControls
- GLTFLoader (um 3D-Modelle laden zu können)
- 3d-model.js (das Skript in dem der Code steht, der das Modell lädt und positioniert)
# AR Core Demo

![alt](https://www.manektech.com/images/logo_with_text.png)

With use of below code you can create AR demo app at your end. You just need to follow below steps 

# Installation & setup
 - Add below depedancies in the build.gradle file. 
```sh
implementation "com.google.ar.sceneform.ux:sceneform-ux:1.4.0"
apply plugin: 'com.google.ar.sceneform.plugin'
```
 - Then sync project. 
 
 //add this permissions in manifest file
 ```sh
<uses-permission android:name="android.permission.CAMERA" />
    <uses-feature
        android:name="android.hardware.camera.ar"
        android:required="true" />
        
// add below metadata
```sh
<meta-data
android:name="com.google.ar.core"
android:value="required" />

- Create  MainActivity Class and ArSceneview like below,
```sh
class MainActivity : AppCompatActivity() {

arFragment.arSceneView.scene.addOnUpdateListener { frameTime ->
            arFragment.onUpdate(frameTime)
            onUpdate()
        }

/**
* @param model The Uri of our 3D sfb file
*/
private fun addObject(model: Uri) {
        val frame = arFragment.arSceneView.arFrame
        val point = getScreenCenter()
        if (frame != null) {
            val hits = frame.hitTest(point.x.toFloat(), point.y.toFloat())
            for (hit in hits) {
                val trackable = hit.trackable
                if (trackable is Plane && trackable.isPoseInPolygon(hit.hitPose)) {
                    placeObject(arFragment, hit.createAnchor(), model)
                    break
                }
            }
        }
    }        

}

/**
* @param your fragment
* @param hit test for ARcore anchor
* @param 3D Model
*/
 private fun placeObject(fragment: ArFragment, anchor: Anchor, model: Uri) {
        ModelRenderable.builder()
                .setSource(fragment.context, model)
                .build()
                .thenAccept {
                    addNodeToScene(fragment, anchor, it)
                }
                .exceptionally {
                    Toast.makeText(this@MainActivity, "Error", Toast.LENGTH_SHORT).show()
                    return@exceptionally null
                }
    }
    
/**
* @param fragment your fragment
* @param anchor ARCore anchor
* @param sceneform Renderable
*/
private fun addNodeToScene(fragment: ArFragment, anchor: Anchor, renderable: ModelRenderable) {
       val anchorNode = AnchorNode(anchor)
       // TransformableNode means the user to move, scale and rotate the model
       val transformableNode = TransformableNode(fragment.transformationSystem)
       transformableNode.renderable = renderable
       transformableNode.setParent(anchorNode)
       fragment.arSceneView.scene.addChild(anchorNode)
       transformableNode.select()
}
```
- You can visit below offical site for get more details link(https://developer.android.com/) 

- Thank you for the reading

- You can read more details on our blog. [Build Augmented Reality (AR) application in Android by Manektech](https://www.manektech.com/blog/ar-with-3d-objects-in-android)

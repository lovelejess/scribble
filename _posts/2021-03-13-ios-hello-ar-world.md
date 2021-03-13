---
title: "iOS - hello ar world!"
date: 2021-03-13 14:28:26
---

> Notes from [Beginning ARKit- Hello Augmented World!](https://www.raywenderlich.com/737368-beginning-arkit/lessons/1)


![sample hello ar world](../images/hello-ar-world.gif)

### How to Create a Sphere at Origin 

```
/// Creates a blue sphere with 5 centimeters radius at the origin
/// It rotates 360 around y-axis and completes a rotation in 8 seconds
func drawSphereAtOrigin() -> SCNNode {
    let sphere = SCNNode(geometry: SCNSphere(radius: 0.05))
    
    // sets the sphere to a flat blue - diffuse = matte
    sphere.geometry?.firstMaterial?.diffuse.contents = UIColor.blue
    // sphere's origin is at (0,0,0)
    sphere.position = SCNVector3(0, 0, 0)
    
    /// rotate (360 degrees around its y-axis) the Sphere in 8 seconds once at the beginning of AR session
    let rotateAction = SCNAction.rotate(by: 360.degreeToRadians(), around: SCNVector3(0, 1, 0), duration: 8)
    sphere.runAction(rotateAction)

    return sphere
}

override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
    
    // Create a session configuration
    let configuration = ARWorldTrackingConfiguration()

    // Run the view's session
    sceneView.session.run(configuration)

    let sphere = drawSphereAtOrigin()
    sceneView.scene.rootNode.addChildNode(sphere)
}

extension Int {
    /// Converts the Int in degrees to radians in CGFloat
    /// - Returns: Radian in CGFloat
    func degreeToRadians() -> CGFloat {
        return CGFloat(self) * CGFloat.pi / 180.0
    }
}

```


### How to Create an Oribiting Ship around Earth 

```
func drawOrbitingShip() -> SCNNode {
    // grab the ship scene
    let scene = SCNScene(named: "art.scnassets/ship.scn")!
    // extract the ship object from the scene
    let ship = (scene.rootNode.childNode(withName: "ship", recursively: false))!
    // ship is 1 meter to right of origin
    ship.position = SCNVector3(1, 0, 0)
    // rotates the ship about the y-axis, so it's facing the same direction as the Earth's rotation
    ship.eulerAngles = SCNVector3(0, 180.degreeToRadians(), 0)
    // scale it down to 1/3 of size since it's large relative to other shapes
    ship.scale = SCNVector3(0.3, 0.3, 0.3)

    return ship
}
    
func drawSphereAtOriginWithImages() -> SCNNode {
    let sphere = SCNNode(geometry: SCNSphere(radius: 0.05))
    
    // sets the sphere to use earth image
    sphere.geometry?.firstMaterial?.diffuse.contents = UIImage(named: "earth")
    
    // sets the sphere to use yellow shine
    sphere.geometry?.firstMaterial?.specular.contents = UIColor.yellow
    // sphere's origin is at (0,0,0)
    sphere.position = SCNVector3(0, 0, 0)
    
    /// rotate (360 degrees around its y-axis) the Sphere in 8 seconds once at the beginning of AR session
    let rotateAction = SCNAction.rotate(by: 360.degreeToRadians(), around: SCNVector3(0, 1, 0), duration: 8)
    /// repeat the rotation
    let rotateForeverAction = SCNAction.repeatForever(rotateAction)
    sphere.runAction(rotateForeverAction)

    return sphere
}

// How to Use It
let sphere = drawSphereAtOriginWithImages()
sceneView.scene.rootNode.addChildNode(sphere)
sphere.addChildNode(drawOrbitingShip())

```


### How to Create a Box at 12:00 High

```
func drawBoxAt1200High() -> SCNNode {
    let box = SCNNode(geometry: SCNBox(width: 0.1, height: 0.1, length: 0.1, chamferRadius: 0.0))
    
    // sets the sphere to a flat white - diffuse = matte
    box.geometry?.firstMaterial?.diffuse.contents = UIColor.orange
    // sets the sphere to a shinyh white - specular = shine
    box.geometry?.firstMaterial?.specular.contents = UIColor.white
    
    // 20 centimeters above origin & 30 centimeters behind origin
    box.position = SCNVector3(0, 0.2, -0.3)
    
    return box
}

// How to Use It
sceneView.scene.rootNode.addChildNode(drawBoxAt1200High())

```


### How to Create a Pyramid at 6:00 Low

```
/// Challenge: Draw a pyramid
/// 10 centimeter high, 10 centimeter wide, 10 centimeter deep
/// green with red highlights
/// at 6'clock - 30 centimeters in front origin and 20 centimeters below origin
func drawPyramidAt600Low() -> SCNNode {
    let pyramid = SCNNode(geometry: SCNPyramid(width: 0.10, height: 0.10, length: 0.10))
    
    pyramid.geometry?.firstMaterial?.diffuse.contents = UIColor.green
    pyramid.geometry?.firstMaterial?.specular.contents = UIColor.red
    
    // 20 centimeters below origin & 30 centimeters in front origin
    pyramid.position = SCNVector3(0, -0.20, 0.30)
    
    return pyramid
}

// How to Use It
sceneView.scene.rootNode.addChildNode(drawPyramidAt600Low())
```

### How to Draw a Cat on a Plane at 9:00

```
/// Challenge: Draws plane with a cat image on its surface
/// 10 centimeter high, 10 centimeter wide
/// 20 centimeters to the left of origin
func drawCatOnAPlaneAt900() -> SCNNode {
    let plane = SCNNode(geometry: SCNPlane(width: 0.10, height: 0.10))
    
    plane.geometry?.firstMaterial?.diffuse.contents = UIImage(named: "cat")
    plane.geometry?.firstMaterial?.isDoubleSided = true

    // sets the plane at centimeters to the left of origin
    plane.position = SCNVector3(-0.20, 0, 0)

    return plane
}

// How to Use It
sceneView.scene.rootNode.addChildNode(drawCatOnAPlaneAt900())
```


### How to Tilt A Cat

```
/// Challenge: Draws plane with a cat image on its surface
/// 10 centimeter high, 10 centimeter wide
/// 20 centimeters to the left of origin
/// 45 degrees clockwise around x axis
/// 20 degrees counterclockwise around y axis
/// 45 degrees counterclockwise around z axis
func tiltTheCat() -> SCNNode {
    let plane = SCNNode(geometry: SCNPlane(width: 0.10, height: 0.10))
    
    plane.geometry?.firstMaterial?.diffuse.contents = UIImage(named: "cat")
    plane.geometry?.firstMaterial?.isDoubleSided = true

    // sets the plane at centimeters to the left of origin
    plane.position = SCNVector3(-0.20, 0, 0)
    
    // 45 degrees clockwise around x axis, 20 degrees counterclockwise around y axis, 45 degrees counterclockwise around z axis
    plane.eulerAngles = SCNVector3(-45.degreeToRadians(), 20.degreeToRadians(), 45.degreeToRadians())

    return plane
}


// How to Use It
sceneView.scene.rootNode.addChildNode(tiltTheCat())
```

### How to Draw a Torus

```
/// Draws a Torus (doughnut) at 3 o'clock
/// Ring Radius = 5 centimeters
/// Pipe Radius = 3 centimeters
/// 20 centimeters to the left of origin
/// 45 degree angle
func drawTorusAt300() -> SCNNode {
    let torus = SCNNode(geometry: SCNTorus(ringRadius: 0.05, pipeRadius: 0.03))
    
    torus.geometry?.firstMaterial?.diffuse.contents = UIColor.red
    torus.geometry?.firstMaterial?.specular.contents = UIColor.white
    
    torus.position = SCNVector3(0.20, 0, 0)

    // sets the z axis to a 45 degree angle
    torus.eulerAngles = SCNVector3(0, 0, 45.degreeToRadians())
    return torus
}

// How to Use It
sceneView.scene.rootNode.addChildNode(drawTorusAt300())
```

### Entire File
```
//
//  ViewController.swift
//  HelloARWorld
//
//  Created by Jess Lê on 3/13/21.
//

import UIKit
import SceneKit
import ARKit

class ViewController: UIViewController, ARSCNViewDelegate {

    @IBOutlet var sceneView: ARSCNView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Set the view's delegate
        sceneView.delegate = self
        
        // Show statistics such as fps and timing information
        sceneView.showsStatistics = true
        
        // activates debugging features
        sceneView.debugOptions = [ARSCNDebugOptions.showWorldOrigin,
                                  ARSCNDebugOptions.showFeaturePoints]
    }
    
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        
        // Create a session configuration
        let configuration = ARWorldTrackingConfiguration()

        // Run the view's session
        sceneView.session.run(configuration)

        let sphere = drawSphereAtOriginWithImages()
        sceneView.scene.rootNode.addChildNode(sphere)
        sphere.addChildNode(drawOrbitingShip())
        sceneView.scene.rootNode.addChildNode(drawBoxAt1200High())
        sceneView.scene.rootNode.addChildNode(drawPyramidAt600Low())
        sceneView.scene.rootNode.addChildNode(drawTorusAt300())
//        sceneView.scene.rootNode.addChildNode(drawCatOnAPlaneAt900())
        sceneView.scene.rootNode.addChildNode(tiltTheCat())
    }
    
    override func viewWillDisappear(_ animated: Bool) {
        super.viewWillDisappear(animated)
        
        // Pause the view's session
        sceneView.session.pause()
    }
    
    
    func drawSphereAtOrigin() -> SCNNode {
        let sphere = SCNNode(geometry: SCNSphere(radius: 0.05))
        
        // sets the sphere to a flat blue - diffuse = matte
        sphere.geometry?.firstMaterial?.diffuse.contents = UIColor.blue
        // sphere's origin is at (0,0,0)
        sphere.position = SCNVector3(0, 0, 0)
        
        /// rotate (360 degrees around its y-axis) the Sphere in 8 seconds once at the beginning of AR session
        let rotateAction = SCNAction.rotate(by: 360.degreeToRadians(), around: SCNVector3(0, 1, 0), duration: 8)
        sphere.runAction(rotateAction)

        return sphere
    }
    
    func drawOrbitingShip() -> SCNNode {
        // grab the ship scene
        let scene = SCNScene(named: "art.scnassets/ship.scn")!
        // extract the ship object from the scene
        let ship = (scene.rootNode.childNode(withName: "ship", recursively: false))!
        // ship is 1 meter to right of origin
        ship.position = SCNVector3(1, 0, 0)
        // rotates the ship about the y-axis, so it's facing the same direction as the Earth's rotation
        ship.eulerAngles = SCNVector3(0, 180.degreeToRadians(), 0)
        // scale it down to 1/3 of size since it's large relative to other shapes
        ship.scale = SCNVector3(0.3, 0.3, 0.3)

        return ship
    }
    
    func drawSphereAtOriginWithImages() -> SCNNode {
        let sphere = SCNNode(geometry: SCNSphere(radius: 0.05))
        
        // sets the sphere to use earth image
        sphere.geometry?.firstMaterial?.diffuse.contents = UIImage(named: "earth")
        
        // sets the sphere to use yellow shine
        sphere.geometry?.firstMaterial?.specular.contents = UIColor.yellow
        // sphere's origin is at (0,0,0)
        sphere.position = SCNVector3(0, 0, 0)
        
        /// rotate (360 degrees around its y-axis) the Sphere in 8 seconds once at the beginning of AR session
        let rotateAction = SCNAction.rotate(by: 360.degreeToRadians(), around: SCNVector3(0, 1, 0), duration: 8)
        /// repeat the rotation
        let rotateForeverAction = SCNAction.repeatForever(rotateAction)
        sphere.runAction(rotateForeverAction)

        return sphere
    }
    
    func drawBoxAt1200High() -> SCNNode {
        let box = SCNNode(geometry: SCNBox(width: 0.1, height: 0.1, length: 0.1, chamferRadius: 0.0))
        
        // sets the sphere to a flat white - diffuse = matte
        box.geometry?.firstMaterial?.diffuse.contents = UIColor.orange
        // sets the sphere to a shinyh white - specular = shine
        box.geometry?.firstMaterial?.specular.contents = UIColor.white
        
        // 20 centimeters above origin & 30 centimeters behind origin
        box.position = SCNVector3(0, 0.2, -0.3)
        
        return box
    }
    
    /// Challenge: Draw a pyramid
    /// 10 centimeter high, 10 centimeter wide, 10 centimeter deep
    /// green with red highlights
    /// at 6'clock - 30 centimeters in front origin and 20 centimeters below origin
    func drawPyramidAt600Low() -> SCNNode {
        let pyramid = SCNNode(geometry: SCNPyramid(width: 0.10, height: 0.10, length: 0.10))
        
        pyramid.geometry?.firstMaterial?.diffuse.contents = UIColor.green
        pyramid.geometry?.firstMaterial?.specular.contents = UIColor.red
        
        // 20 centimeters below origin & 30 centimeters in front origin
        pyramid.position = SCNVector3(0, -0.20, 0.30)
        
        return pyramid
    }
    
    /// Challenge: Draws plane with a cat image on its surface
    /// 10 centimeter high, 10 centimeter wide
    /// 20 centimeters to the left of origin
    func drawCatOnAPlaneAt900() -> SCNNode {
        let plane = SCNNode(geometry: SCNPlane(width: 0.10, height: 0.10))
        
        plane.geometry?.firstMaterial?.diffuse.contents = UIImage(named: "cat")
        plane.geometry?.firstMaterial?.isDoubleSided = true
    
        // sets the plane at centimeters to the left of origin
        plane.position = SCNVector3(-0.20, 0, 0)

        return plane
    }
    
    /// Draws a Torus (doughnut) at 3 o'clock
    /// Ring Radius = 5 centimeters
    /// Pipe Radius = 3 centimeters
    /// 20 centimeters to the left of origin
    /// 45 degree angle
    func drawTorusAt300() -> SCNNode {
        let torus = SCNNode(geometry: SCNTorus(ringRadius: 0.05, pipeRadius: 0.03))
        
        torus.geometry?.firstMaterial?.diffuse.contents = UIColor.red
        torus.geometry?.firstMaterial?.specular.contents = UIColor.white
        
        torus.position = SCNVector3(0.20, 0, 0)

        // sets the z axis to a 45 degree angle
        torus.eulerAngles = SCNVector3(0, 0, 45.degreeToRadians())
        return torus
    }
    
    /// Challenge: Draws plane with a cat image on its surface
    /// 10 centimeter high, 10 centimeter wide
    /// 20 centimeters to the left of origin
    /// 45 degrees clockwise around x axis
    /// 20 degrees counterclockwise around y axis
    /// 45 degrees counterclockwise around z axis
    func tiltTheCat() -> SCNNode {
        let plane = SCNNode(geometry: SCNPlane(width: 0.10, height: 0.10))
        
        plane.geometry?.firstMaterial?.diffuse.contents = UIImage(named: "cat")
        plane.geometry?.firstMaterial?.isDoubleSided = true
    
        // sets the plane at centimeters to the left of origin
        plane.position = SCNVector3(-0.20, 0, 0)
        
        // 45 degrees clockwise around x axis, 20 degrees counterclockwise around y axis, 45 degrees counterclockwise around z axis
        plane.eulerAngles = SCNVector3(-45.degreeToRadians(), 20.degreeToRadians(), 45.degreeToRadians())

        return plane
    }

    // MARK: - ARSCNViewDelegate
    
/*
    // Override to create and configure nodes for anchors added to the view's session.
    func renderer(_ renderer: SCNSceneRenderer, nodeFor anchor: ARAnchor) -> SCNNode? {
        let node = SCNNode()
     
        return node
    }
*/
    
    func session(_ session: ARSession, didFailWithError error: Error) {
        // Present an error message to the user
        
    }
    
    func sessionWasInterrupted(_ session: ARSession) {
        // Inform the user that the session has been interrupted, for example, by presenting an overlay
        
    }
    
    func sessionInterruptionEnded(_ session: ARSession) {
        // Reset tracking and/or remove existing anchors if consistent tracking is required
        
    }
}

extension Int {
    /// Converts the Int in degrees to radians in CGFloat
    /// - Returns: Radian in CGFloat
    func degreeToRadians() -> CGFloat {
        return CGFloat(self) * CGFloat.pi / 180.0
    }
}

```
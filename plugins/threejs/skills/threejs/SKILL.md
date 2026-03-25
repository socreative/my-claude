---
name: threejs
description: Expert knowledge of Three.js, the JavaScript 3D library for WebGL. Use this skill when the user asks to build 3D graphics, create WebGL scenes, work with cameras, lights, materials, geometries, animations, or post-processing in the browser.
---

# Three.js 3D Graphics Expert

You are an expert in Three.js, the JavaScript 3D library for creating WebGL-based 3D graphics in the browser.

## Core Concepts

### The Three.js Workflow

Every Three.js application follows this fundamental pattern:

1. **Scene** - Container that holds all 3D objects, lights, and cameras
2. **Camera** - Defines the viewpoint for rendering
3. **Renderer** - Draws the scene from the camera's perspective
4. **Geometry** - Defines the shape/mesh data
5. **Material** - Defines surface appearance
6. **Mesh** - Combines geometry + material into a renderable object
7. **Animation Loop** - Continuously renders and updates the scene

### Basic Setup Pattern

```javascript
// 1. Create scene
const scene = new THREE.Scene();

// 2. Setup camera
const camera = new THREE.PerspectiveCamera(
  75,                                      // Field of view
  window.innerWidth / window.innerHeight,  // Aspect ratio
  0.1,                                     // Near clipping plane
  1000                                     // Far clipping plane
);
camera.position.z = 5;

// 3. Create renderer
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setPixelRatio(window.devicePixelRatio);
document.body.appendChild(renderer.domElement);

// 4. Create object (geometry + material = mesh)
const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshStandardMaterial({ color: 0x00ff00 });
const mesh = new THREE.Mesh(geometry, material);
scene.add(mesh);

// 5. Add lights
const light = new THREE.DirectionalLight(0xffffff, 1);
light.position.set(5, 5, 5);
scene.add(light);
scene.add(new THREE.AmbientLight(0xffffff, 0.5));

// 6. Animation loop
function animate() {
  requestAnimationFrame(animate);
  mesh.rotation.x += 0.01;
  mesh.rotation.y += 0.01;
  renderer.render(scene, camera);
}
animate();

// 7. Handle window resize
window.addEventListener('resize', () => {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});
```

## API Categories

### Cameras

**PerspectiveCamera** - Realistic perspective (most common)
- Parameters: FOV, aspect ratio, near plane, far plane
- Use for: Games, realistic scenes

**OrthographicCamera** - No perspective distortion
- Parameters: left, right, top, bottom, near, far
- Use for: 2D games, technical drawings, UI

**CubeCamera** - 6-direction rendering for environment maps
**ArrayCamera** - Multiple viewports (split-screen)

### Lights

**AmbientLight** - Uniform lighting from all directions
- No shadows, affects all objects equally
- Use for: Base ambient illumination

**DirectionalLight** - Parallel rays (sun-like)
- Supports shadows
- Use for: Outdoor scenes, sunlight

**PointLight** - Omnidirectional from a point (light bulb)
- Supports shadows
- Use for: Indoor lighting, lamps

**SpotLight** - Cone-shaped directional light
- Supports shadows, adjustable angle and penumbra
- Use for: Stage lighting, flashlights

**HemisphereLight** - Gradient between sky and ground color
- No shadows, very efficient
- Use for: Outdoor ambient with color variation

**RectAreaLight** - Rectangular area light (realistic)
- No shadows, only works with MeshStandardMaterial/MeshPhysicalMaterial
- Use for: Windows, LED panels

### Materials

**MeshBasicMaterial** - Unlit, flat color
- Fastest, no lighting calculations
- Use for: UI elements, debugging

**MeshLambertMaterial** - Diffuse reflection only
- Good performance, matte appearance
- Use for: Non-shiny surfaces

**MeshPhongMaterial** - Specular highlights (Phong shading)
- Good balance of performance and quality
- Use for: Shiny surfaces, general purpose

**MeshStandardMaterial** - PBR (Physically Based Rendering)
- Metalness and roughness workflow
- Use for: Realistic materials (recommended for most cases)

**MeshPhysicalMaterial** - Advanced PBR
- Adds clearcoat, transmission, sheen
- Use for: Car paint, glass, fabrics

**ShaderMaterial** - Custom GLSL shaders
- Full control, requires shader knowledge
- Use for: Custom effects, advanced materials

**LineBasicMaterial** / **LineDashedMaterial** - For line rendering
**PointsMaterial** - For point clouds
**SpriteMaterial** - For billboards/sprites

### Geometries

**Primitives:**
- **BoxGeometry** - Cube/rectangular prism
- **SphereGeometry** - Sphere
- **PlaneGeometry** - Flat plane
- **CylinderGeometry** - Cylinder
- **ConeGeometry** - Cone
- **TorusGeometry** - Donut shape
- **TorusKnotGeometry** - 3D knot

**Advanced:**
- **ExtrudeGeometry** - Extrude 2D shapes into 3D
- **LatheGeometry** - Surface of revolution
- **TubeGeometry** - Tube along a path
- **ShapeGeometry** - 2D shapes
- **BufferGeometry** - Custom geometry (most efficient)

### Objects

**Mesh** - Visible object (geometry + material)
**Group** - Container for organizing multiple objects
**Line** / **LineSegments** - Line rendering
**Points** - Point cloud rendering
**Sprite** - 2D billboard (always faces camera)
**SkinnedMesh** - Mesh with skeletal animation
**InstancedMesh** - Efficient rendering of many identical objects
**LOD (Level of Detail)** - Automatic detail switching based on distance

### Loaders

**GLTFLoader** - glTF/glB format (recommended for 3D models)
- Industry standard, supports animations, materials, PBR
- Use for: Most 3D models

**FBXLoader** - Autodesk FBX format
**OBJLoader** - Wavefront OBJ (geometry only)
**TextureLoader** - Load image textures (JPG, PNG)
**CubeTextureLoader** - Skybox/environment maps
**FontLoader** - Load fonts for TextGeometry
**AudioLoader** - Load audio files for 3D audio

### Textures

**Texture Types:**
- **Color/Diffuse Map** - Base color
- **Normal Map** - Surface detail without geometry
- **Bump Map** - Height-based surface detail
- **Displacement Map** - Actual geometry displacement
- **Roughness Map** - Surface roughness (PBR)
- **Metalness Map** - Metallic properties (PBR)
- **AO Map (Ambient Occlusion)** - Shadowing in crevices
- **Environment Map** - Reflections and lighting

**Texture Settings:**
```javascript
const texture = textureLoader.load('texture.jpg');
texture.wrapS = THREE.RepeatWrapping; // U direction
texture.wrapT = THREE.RepeatWrapping; // V direction
texture.repeat.set(4, 4);
texture.minFilter = THREE.LinearMipmapLinearFilter;
texture.magFilter = THREE.LinearFilter;
texture.anisotropy = renderer.capabilities.getMaxAnisotropy();
```

### Animation

**AnimationMixer** - Controls animation playback
**AnimationClip** - Animation data
**AnimationAction** - Running animation instance
**KeyframeTrack** - Timeline data for properties

```javascript
const mixer = new THREE.AnimationMixer(mesh);
const action = mixer.clipAction(animationClip);
action.play();

// In animation loop
const clock = new THREE.Clock();
function animate() {
  const delta = clock.getDelta();
  mixer.update(delta);
  renderer.render(scene, camera);
  requestAnimationFrame(animate);
}
```

### Controls

**OrbitControls** - Mouse/touch orbit, zoom, pan (most common)
**FlyControls** - Flight simulator-style
**FirstPersonControls** - FPS-style movement
**TrackballControls** - Unrestricted rotation
**PointerLockControls** - FPS pointer lock
**TransformControls** - Gizmo for moving objects

### Helpers

**AxesHelper** - RGB axes (X=red, Y=green, Z=blue)
**GridHelper** - Ground plane grid
**CameraHelper** - Visualize camera frustum
**DirectionalLightHelper** - Show light direction
**SpotLightHelper** - Show spotlight cone
**BoxHelper** - Bounding box visualization
**ArrowHelper** - Direction arrow
**SkeletonHelper** - Visualize bone structure

### Math Utilities

**Vector2, Vector3, Vector4** - Vector operations
**Quaternion** - Rotation (avoids gimbal lock)
**Euler** - Euler angles (rotation in degrees/radians)
**Matrix3, Matrix4** - Transformation matrices
**Box3** - 3D bounding box
**Sphere** - Bounding sphere
**Plane** - Mathematical plane
**Ray** - Ray for raycasting
**Color** - Color manipulation

## Common Patterns

### Raycasting (Mouse Picking)

```javascript
const raycaster = new THREE.Raycaster();
const mouse = new THREE.Vector2();

window.addEventListener('click', (event) => {
  mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
  mouse.y = -(event.clientY / window.innerHeight) * 2 + 1;

  raycaster.setFromCamera(mouse, camera);
  const intersects = raycaster.intersectObjects(scene.children);

  if (intersects.length > 0) {
    console.log('Clicked:', intersects[0].object);
  }
});
```

### Loading 3D Models

```javascript
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';

const loader = new GLTFLoader();
loader.load('model.glb', (gltf) => {
  const model = gltf.scene;
  scene.add(model);

  // Play animations if available
  if (gltf.animations.length > 0) {
    const mixer = new THREE.AnimationMixer(model);
    gltf.animations.forEach((clip) => {
      mixer.clipAction(clip).play();
    });
  }
}, undefined, (error) => {
  console.error('Loading error:', error);
});
```

### Post-Processing Effects

```javascript
import { EffectComposer } from 'three/addons/postprocessing/EffectComposer.js';
import { RenderPass } from 'three/addons/postprocessing/RenderPass.js';
import { UnrealBloomPass } from 'three/addons/postprocessing/UnrealBloomPass.js';

const composer = new EffectComposer(renderer);
composer.addPass(new RenderPass(scene, camera));
composer.addPass(new UnrealBloomPass(
  new THREE.Vector2(window.innerWidth, window.innerHeight),
  1.5,  // strength
  0.4,  // radius
  0.85  // threshold
));

// In animation loop, use composer instead of renderer
composer.render();
```

### Instanced Rendering (Performance)

```javascript
const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshStandardMaterial({ color: 0x00ff00 });
const count = 10000;

const mesh = new THREE.InstancedMesh(geometry, material, count);

const matrix = new THREE.Matrix4();
for (let i = 0; i < count; i++) {
  matrix.setPosition(
    Math.random() * 100 - 50,
    Math.random() * 100 - 50,
    Math.random() * 100 - 50
  );
  mesh.setMatrixAt(i, matrix);
}
scene.add(mesh);
```

## Performance Optimization

1. **Use InstancedMesh** for many identical objects
2. **Merge geometries** when possible with BufferGeometryUtils
3. **Use texture atlases** to reduce draw calls
4. **Implement frustum culling** - objects outside view aren't rendered (automatic)
5. **Use LOD** for distant objects
6. **Dispose of unused resources**: `geometry.dispose()`, `material.dispose()`, `texture.dispose()`
7. **Use lower-poly models** for background objects
8. **Limit light count** - each light adds computational cost
9. **Use shadowMap carefully** - shadows are expensive
10. **Profile with** `renderer.info` to see render stats

## Common Use Cases

### Games
- 3D environments with OrbitControls or FirstPersonControls
- Physics integration (Rapier, Cannon.js)
- Character animation with SkinnedMesh
- Raycasting for interactions

### Data Visualization
- 3D charts and graphs with custom geometries
- Particle systems with Points
- Interactive exploration with OrbitControls

### Product Configurators
- glTF model loading
- Material/texture swapping
- Camera animations
- Environment maps for realistic reflections

### WebXR (VR/AR)
- WebXRManager for VR/AR sessions
- Controller input handling
- Hand tracking
- Immersive 3D experiences

### Architectural Visualization
- Orthographic camera for technical views
- Realistic materials with MeshPhysicalMaterial
- Lightmaps for baked lighting
- High-quality shadows

## Best Practices

1. **Always dispose** of geometries, materials, and textures when removing objects
2. **Use requestAnimationFrame** for animation loops, not setInterval
3. **Update matrices** manually with `mesh.updateMatrix()` when needed
4. **Use Clock** for delta time to ensure consistent animation speed
5. **Set pixelRatio** properly: `renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))`
6. **Handle window resize** to update camera and renderer
7. **Use development mode** for debugging, production mode for deployment
8. **Test performance** on target devices early
9. **Use appropriate materials** - not everything needs PBR
10. **Structure scene hierarchically** with Groups for easier management

## Debugging Tips

```javascript
// Show wireframes
material.wireframe = true;

// Show normals
const helper = new THREE.VertexNormalsHelper(mesh, 1, 0xff0000);
scene.add(helper);

// Log renderer info
console.log(renderer.info);

// Check bounding boxes
const box = new THREE.Box3().setFromObject(mesh);
console.log('Bounding box:', box);
```

## Resources

**Official Documentation**: https://threejs.org/docs/
**Examples**: https://threejs.org/examples/
**Manual**: https://threejs.org/manual/
**Editor**: https://threejs.org/editor/
**GitHub**: https://github.com/mrdoob/three.js

## Installation

```bash
# npm
npm install three

# Import in JavaScript
import * as THREE from 'three';
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
```

## Response Format

When helping with Three.js:

1. Identify the core concept (Scene, Camera, Renderer, etc.)
2. Provide working code examples
3. Explain performance implications
4. Suggest appropriate materials and techniques
5. Link to relevant documentation or examples
6. Consider browser compatibility and mobile performance
7. Recommend best practices for production use

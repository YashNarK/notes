# Three JS

## Table of Contents

- [Three JS](#three-js)

---

**Three.js** is a popular open-source JavaScript libraryused to create and display 3D graphics in web browsers. It enables developers to build complex 3D scenes, animations, and visualizations directly in the browser without requiring plugins. Three.js leverages WebGL (Web Graphics Library), which is a JavaScript API for rendering 2D and 3D graphics, to provide a high-level abstraction for easier development.

### Key Features of Three.js:

1. **Ease of Use**: Provides simplified APIs for creating 3D objects, animations, and interactions compared to using raw WebGL.
2. **Cross-Platform**: Works in modern browsers without additional installations.
3. **Rich Object Support**:
   - Primitives: Spheres, cubes, planes, and other basic 3D shapes.
   - Custom geometries: Create complex models by defining vertices and faces.
4. **Material System**: Offers various materials like `MeshBasicMaterial`, `MeshPhongMaterial`, and `MeshStandardMaterial` for realistic rendering with lighting and textures.
5. **Lighting**: Supports different types of lights, such as ambient, directional, point, and spotlights, to create realistic effects.
6. **Textures**: Allows applying textures to objects for more detailed visuals.
7. **Animations**: Built-in support for animations and morphing of objects.
8. **Camera Controls**: Multiple camera types (Perspective, Orthographic) and interaction controls (e.g., orbit controls).
9. **Shadows and Effects**: Provides advanced rendering features like shadows, post-processing effects, and environment mapping.
10. **Extensibility**: Supports importing 3D models from formats like glTF, OBJ, FBX, and more.

### Common Use Cases:

1. **Games**: Build immersive browser-based 3D games.
2. **Data Visualization**: Create interactive 3D graphs, charts, or geographic maps.
3. **Product Design**: Showcase 3D models of products with interactive features.
4. **Virtual Reality (VR)**: Integrates with WebXR to build VR experiences.
5. **Architecture and Real Estate**: Visualize architectural designs or real estate properties.

### Example Code:

Here’s a simple example to create a spinning cube:

```js
import React, { useEffect, useRef } from "react";
// Import React and necessary hooks: `useEffect` for side effects and `useRef` for creating a DOM reference.
import * as THREE from "three";
// Import the Three.js library for creating 3D scenes, geometries, cameras, and rendering.

const SpinningCube: React.FC = () => {
  // Define a functional component using React.FC (TypeScript's type for function components).

  const mountRef = useRef<HTMLDivElement | null>(null);
  // Create a ref for the container div where the Three.js renderer will mount.
  // The ref can initially be `null` and will later reference an `HTMLDivElement`.

  useEffect(() => {
    if (!mountRef.current) return;
    // Ensure that the ref is not null before proceeding. This handles the possibility that the DOM element doesn't exist yet.

    // Create the Three.js scene
    const scene = new THREE.Scene();
    // The scene acts as a container for all 3D objects, lights, and cameras.

    // Create a perspective camera
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    // - 75: Field of view (degrees).
    // - window.innerWidth / window.innerHeight: Aspect ratio based on the browser window dimensions.
    // - 0.1: Near clipping plane (objects closer than this won't be rendered).
    // - 1000: Far clipping plane (objects farther than this won't be rendered).

    const renderer = new THREE.WebGLRenderer();
    // Create a WebGL renderer to handle rendering the 3D scene using WebGL.

    renderer.setSize(window.innerWidth, window.innerHeight);
    // Set the renderer's output size to match the browser's window size.

    mountRef.current.appendChild(renderer.domElement);
    // Append the renderer's canvas (DOM element) to the `div` referenced by `mountRef`.

    // Create a cube geometry
    const geometry = new THREE.BoxGeometry();
    // Defines a 3D cube shape.

    const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
    // Create a material with a green color. MeshBasicMaterial doesn't respond to lighting.

    const cube = new THREE.Mesh(geometry, material);
    // Combine the geometry and material into a mesh (the visual representation of the cube).

    scene.add(cube);
    // Add the cube to the scene so it can be rendered.

    // Move the camera back and up slightly to view the grid better
    camera.position.set(5, 5, 10); // Set the camera position to x=5, y=5, z=10
    camera.lookAt(0, 0, 0); // Make the camera look at the origin (0, 0, 0)

    // Add a grid helper to the scene
    const gridHelper = new THREE.GridHelper(20, 20);
    // - 20: The size of the grid.
    // - 20: The number of divisions.
    scene.add(gridHelper);

    // Add an axes helper to the scene
    const axesHelper = new THREE.AxesHelper(10);
    // - 10: The size of the axes.
    scene.add(axesHelper);
    // Helper function to create a text sprite
    const createTextSprite = (text: string, color: string) => {
      const canvas = document.createElement("canvas");
      const context = canvas.getContext("2d");
      if (!context) return new THREE.Sprite();

      // Set canvas size and style
      canvas.width = 256;
      canvas.height = 128;
      context.fillStyle = color;
      context.font = "30px Arial";
      context.textAlign = "center";
      context.textBaseline = "middle";
      context.fillText(text, canvas.width / 2, canvas.height / 2);

      // Create a texture from the canvas
      const texture = new THREE.CanvasTexture(canvas);
      const material = new THREE.SpriteMaterial({ map: texture });
      return new THREE.Sprite(material);
    };

    // Create and position axis labels
    const xLabel = createTextSprite("X", "red");
    xLabel.position.set(11, 0, 0);
    scene.add(xLabel);

    const yLabel = createTextSprite("Y", "green");
    yLabel.position.set(0, 11, 0);
    scene.add(yLabel);

    const zLabel = createTextSprite("Z", "blue");
    zLabel.position.set(0, 0, 11);
    scene.add(zLabel);

    const animate = () => {
      // Define the animation loop function.
      requestAnimationFrame(animate);
      // Schedule the animate function to run on the next frame (provides smooth animation).

      cube.rotation.x += 0.01;
      // Rotate the cube slightly along the x-axis.

      cube.rotation.y += 0.01;
      // Rotate the cube slightly along the y-axis.

      renderer.render(scene, camera);
      // Render the scene from the camera's perspective.
    };

    animate();
    // Start the animation loop.

    return () => {
      // Cleanup function to execute when the component unmounts.
      if (mountRef.current) {
        // Ensure the DOM element exists before trying to remove the canvas.
        mountRef.current.removeChild(renderer.domElement);
        // Remove the canvas element added by the renderer.
      }
      renderer.dispose();
      // Dispose of the renderer to free up GPU resources and prevent memory leaks.
    };
  }, []);
  // The empty dependency array ensures the effect runs only once when the component mounts.

  return <div ref={mountRef} style={{ width: "100vw", height: "100vh" }} />;
  // Render a `div` that acts as the container for the Three.js canvas.
  // The `ref` is attached to this `div` so the canvas can be mounted here.
};

export default SpinningCube;
// Export the component so it can be used in other parts of the application.
```

### Getting Started:

To begin using Three.js:

1. Install via npm: `npm install three`.
2. Or use a CDN: `<script src="https://cdn.jsdelivr.net/npm/three@latest/build/three.min.js"></script>`.
3. Explore the official [Three.js documentation](https://threejs.org/docs/) and examples.

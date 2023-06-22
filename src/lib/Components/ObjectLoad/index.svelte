<script>
    import { onMount } from 'svelte'
    import * as THREE from 'three'
    import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
    import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'
    export let id = 'id'

    // GLTF models 
    // 1. gLTF 
    // .gltf file (all the data for the model, geometry and such), 
    // .bin file and 
    // .png file with the unrolled texture of the whole model
    // 2. gLTF-Binary 
    // A single .glb file - it's a binary file and contains all the information 
    // about the model, texture and everything
    // But you can't modify anytihng like the texture after it's been exported
    // 3. gLTF-Draco 
    // Like the default with multiple files but the buffer data is compressed 
    // so it's much lighter 
    // 4. gLTF-Embedded 
    // just one file with .gltf extension but it's just one file, it's json.
    // usually don't use this type 
    

    const modelUrl = './models/glb/piggy.glb'
    // './models/glb_camera_light/piggy.glb'
    // './models/glb_camera_light/truck.glb'
    // './models/glb/truck.glb'
    // './models/Duck/glTF/Duck.gltf',
    // './models/Duck/glTF-Binary/Duck.glb',
    // './models/glb/piggy.glb',
    // './models/FlightHelmet/glTF/FlightHelmet.gltf',
  
    // Sizes
    const sizes = {
      width: 1100,
      height: 700
    }
    let canvas
    let scene 
    let geometry
    let material
    let mesh
    let directionalLight
    let ambientLight
    let camera
    let controls
    let renderer
    let gltfLoader
  
  
    onMount(() => {
      // Canvas
      canvas = document.getElementById(`${id}`)
  
      // Scene
      scene = new THREE.Scene()
  
      // Geometry
      geometry = new THREE.BoxGeometry(1, 1, 1, 10, 10, 10)
  
      // Material
      material = new THREE.MeshStandardMaterial({ color: 'aqua',})
  
      // Mesh
      mesh = new THREE.Mesh(geometry, material)
    //   scene.add(mesh)

      // Model 
      gltfLoader = new GLTFLoader()
      gltfLoader.load(
        // './models/Duck/glTF/Duck.gltf',
        // './models/Duck/glTF-Binary/Duck.glb',
        // './models/glb/piggy.glb',
        // './models/FlightHelmet/glTF/FlightHelmet.gltf',
        modelUrl,

        (gltf) => {
            console.log(gltf)
            // Option 1
            // scene.add(gltf.scene.children[0])
            // Option 2
            // while (gltf.scene.children.length > 0) {
            //     scene.add(gltf.scene.children[0])
            // }
            // Option 3
            // const children = [...gltf.scene.children]
            // for (const child of children) {
            //     scene.add(child)
            // }
            // Option 4
            scene.add(gltf.scene)
        },
        () => {
            console.log('progress')
        },
        () => {
            console.log('error')
        },
      )
  
      // Light
      directionalLight = new THREE.DirectionalLight(0xffffff, 0.6)
      directionalLight.castShadow = true
      directionalLight.shadow.mapSize.set(1024, 1024)
      directionalLight.shadow.camera.far = 15
      directionalLight.shadow.camera.left = - 7
      directionalLight.shadow.camera.top = 7
      directionalLight.shadow.camera.right = 7
      directionalLight.shadow.camera.bottom = - 7
      directionalLight.position.set(5, 5, 5)
      //scene.add( directionalLight );
      ambientLight = new THREE.AmbientLight(0xffffff, 0.8)
      scene.add( ambientLight );
  
      // Camera
      camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height, 0.1, 100)
      camera.position.set(2, 2, 2)
    //   camera.zoom = 0
      scene.add(camera)
  
      // Controls
      controls = new OrbitControls(camera, canvas)
      controls.target.set(0, 0.75, 0)
      controls.enableDamping = true
      //controls.enableZoom = false
  
      // Renderer
      renderer = new THREE.WebGLRenderer({
          canvas: canvas,
          antialias: true
      })
      renderer.setSize(sizes.width, sizes.height)
      renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
  
      ////////////////////////////////////////
      ////////////// Events //////////////////
      ////////////////////////////////////////
      // Animate
      const tick = () =>
      {
          // Update controls
          controls.update()
  
          // Render
          renderer.render(scene, camera)
  
          // Call tick again on the next frame
          window.requestAnimationFrame(tick)
      }
  
      tick()
    })
  
  </script>
  
  <div class='section'>
    <canvas class='webgl' id={id}></canvas>
  </div>
  
  
  <style>
  </style>
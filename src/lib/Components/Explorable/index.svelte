<script>
  import { onMount } from 'svelte'
  import * as THREE from 'three'
  import { TrackballControls } from 'three/examples/jsm/controls/TrackballControls.js'
  export let id

  onMount(() => {
    // Sizes
    const sizes = {
      width: 1100,
      height: 700
    }

    // Canvas
    const canvas = document.getElementById(`${id}`)

    // Scene
    const scene = new THREE.Scene()

    // Geometry
    const geometry = new THREE.PlaneGeometry( 1.5, 1.5 );

    // Material
    const material = new THREE.MeshBasicMaterial({ color: 'hotpink', transparent: true, opacity: 0.9, wireframe: false, side: THREE.DoubleSide })

    // Meshes
    // Left
    const mesh1 = new THREE.Mesh(geometry, material)
    mesh1.position.set(-1.5, 0, 7)
    mesh1.rotation.y = Math.PI/6
    scene.add(mesh1)

    const mesh3 = new THREE.Mesh(geometry, material)
    mesh3.position.set(-1.5, 0, 5)
    mesh3.rotation.y = Math.PI/6
    scene.add(mesh3)

    const mesh5 = new THREE.Mesh(geometry, material)
    mesh5.position.set(-1.5, 0, 1)
    mesh5.rotation.y = Math.PI/6
    scene.add(mesh5)

    // Right
    const mesh2 = new THREE.Mesh(geometry, material)
    mesh2.position.set(1.5, 0.1, 6)
    mesh2.rotation.y = -Math.PI/6
    scene.add(mesh2)

    const mesh4 = new THREE.Mesh(geometry, material)
    mesh4.position.set(1.5, 0.1, 2)
    mesh4.rotation.y = -Math.PI/6
    scene.add(mesh4)

    // Light
    const directionalLight = new THREE.DirectionalLight( 0xffffff, 0.9 );
    directionalLight.position.set(0.6, 0.2, 0)
    scene.add( directionalLight );
    const ambientLight = new THREE.AmbientLight( 0x404040 ); // soft white light
    scene.add( ambientLight );

    // Camera
    const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height, 0.01, 12)
    camera.position.set(0, 0, 10)
    scene.add(camera)

    // Controls
    const controls = new TrackballControls(camera, canvas)
    controls.enableDamping = true
    //controls.enableZoom = false

    // Renderer
    const renderer = new THREE.WebGLRenderer({
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
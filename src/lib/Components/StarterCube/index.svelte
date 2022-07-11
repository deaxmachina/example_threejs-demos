<script>
  import { onMount } from 'svelte'
  import * as THREE from 'three'
  import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
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
    const geometry = new THREE.BoxGeometry(1, 1, 1, 10, 10, 10)

    // Material
    const material = new THREE.MeshStandardMaterial({ color: 'hotpink',})

    // Mesh
    const mesh = new THREE.Mesh(geometry, material)
    scene.add(mesh)

    // Light
    const directionalLight = new THREE.DirectionalLight( 0xffffff, 0.9 );
    directionalLight.position.set(0.6, 0.2, 0)
    scene.add( directionalLight );
    const ambientLight = new THREE.AmbientLight( 0x404040 ); // soft white light
    scene.add( ambientLight );

    // Camera
    const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height, 0.1, 100)
    camera.position.set(1, 0.3, 1.5)
    scene.add(camera)

    // Controls
    const controls = new OrbitControls(camera, canvas)
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
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
    //const geometry = new THREE.IcosahedronGeometry(0.7, 10)
    //const geometry = new THREE.TorusKnotGeometry( 1, 3, 200, 44 );
    const geometry = new THREE.BoxGeometry( 1, 1, 1, 100, 10, 10 );

    // Material
    // Material 
    const material = new THREE.PointsMaterial({
        color: 'hotpink',
        size: 0.01,
        sizeAttenuation: true,
        transparent: true,
        depthWrite: false,
    })

    // Mesh
    const mesh = new THREE.Points(geometry, material)
    scene.add(mesh)

    // Camera
    const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height, 0.1, 100)
    camera.position.set(0.25, 0, 1.3)
    scene.add(camera)

    // Controls
    const controls = new OrbitControls(camera, canvas)
    controls.enableDamping = true
    controls.enableZoom = false

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

    // Resize 
    window.addEventListener('resize', () =>
    {
        // Update sizes
        // sizes.width = window.innerWidth
        // sizes.height = window.innerHeight

        // Update camera
        camera.aspect = sizes.width / sizes.height
        camera.updateProjectionMatrix()

        // Update renderer
        renderer.setSize(sizes.width, sizes.height)
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
    })

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
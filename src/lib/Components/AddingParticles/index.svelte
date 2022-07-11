<script>
  import { onMount } from 'svelte'
  import * as THREE from 'three'
  import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
  export let id
  import gsap from 'gsap'


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

    /************************************
    ************** Object ***************
    ************************************/
    const MAX_POINTS = 5000

    // Geometry 
    const geometry = new THREE.BufferGeometry()
    const positions = new Float32Array(MAX_POINTS * 3)
    geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3))
    // draw range
    const drawCount = 100 
    geometry.setDrawRange(0, drawCount) // draw from 0 to 100 only 

    // Material
    const material = new THREE.PointsMaterial({ 
        color: '#8448C1',
        size: 0.2,
        sizeAttenuation: true,
        transparent: true,
        depthWrite: false
    })

    // Points 
    const points = new THREE.Points(geometry, material)
    scene.add(points)

    // Next we'll randomly add points
    // Note that we could have done this at the point of setting positions too
    const positionsFromPoints = points.geometry.attributes.position.array;
    for ( let i = 0; i <= MAX_POINTS; i ++ ) {
        const i3 = i * 3
        positionsFromPoints[i3 + 0] = ( Math.random() - 0.5 ) * 30
        positionsFromPoints[i3 + 1] = ( Math.random() - 0.5 ) * 30
        positionsFromPoints[i3 + 2] = ( Math.random() - 0.5 ) * 30
    }

    /************************************
    ************** Camera ***************
    ************************************/
    const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height, 0.1, 100)
    camera.position.z = 30
    scene.add(camera)

    /************************************
    ************* Controls **************
    ************************************/
    const controls = new OrbitControls(camera, canvas)
    controls.enableDamping = true
    controls.enableZoom = false

    /************************************
    ************* Renderer **************
    ************************************/
    const renderer = new THREE.WebGLRenderer({
        canvas: canvas
    })
    renderer.setSize(sizes.width, sizes.height)
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
    renderer.render(scene, camera);

    /************************************
    ********** Functionality ************
    ************************************/
    // Animate
    const clock = new THREE.Clock()
    let drawCountNew = drawCount
    const tick = () =>
    {
        const elapsedTime = clock.getElapsedTime()

        // Update the particles - add more 
        // If you want to change the number of points rendered after the first render
        drawCountNew = ( drawCountNew + 1 ) % MAX_POINTS;
        points.geometry.setDrawRange( 0, drawCountNew );

        // If you want to change the position data values after the first render, you need to set the needsUpdate flag
        // We don't need this here as we're not changing the positions but leaving in for reference
        points.geometry.attributes.position.needsUpdate = true; // required after the first render
        points.geometry.computeBoundingBox(); // good to have in case
        points.geometry.computeBoundingSphere(); // good to have in case

        // Update controls
        controls.update()

        // Render
        renderer.render(scene, camera)

        // Call tick again on the next frame
        window.requestAnimationFrame(tick)
    }

    const btnStart = document.querySelector('.start')
    btnStart.addEventListener('click', () => {
      tick()
    })

  })

</script>

<div class='section'>
  <button class='start'>start</button>
  <canvas class='webgl' id={id}></canvas>
</div>


<style>

  .section {
    position: relative;
  }

  button {
      position: absolute;
      top: 10px;
      left: 10px;
      outline: none;
      z-index: 100; 
      padding: 10px 12px;
      border: none;
      font-size: 1rem;
      border-radius: 0px;
      font-weight: bold;
      color: rgb(143, 121, 153);
      cursor: pointer;
    }

    button:hover {
        background-color: rebeccapurple;
        color: white;
    }
</style>
<script>
  import { onMount } from 'svelte'
  import * as THREE from 'three'
  import { DragControls } from 'three/examples/jsm/controls/DragControls'
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

    /************************************
    ************** Object ***************
    ************************************/
    // Geometry
    const geometry = new THREE.BoxGeometry(1, 1, 1)

    // Multiple meshes with some random parameters
    const numMeshes = 30
    const meshes = []
    for (let i = 0; i < numMeshes; i++) {
        const x = (Math.random() - 0.5)*4
        const y = (Math.random() - 0.5)*2.5
        const z = (Math.random() - 0.5)
        // Need a separate material for the highlight to work on drag
        const material = new THREE.MeshBasicMaterial({ 
            // color: new THREE.Color(1, 0, 0.5), // same colours
            color: new THREE.Color(Math.random(), 0.2, Math.max(0.6, Math.random())), // different random colours
            transparent: true, 
            opacity: 0.9 
        })

        const mesh = new THREE.Mesh(geometry, material)
        mesh.position.x = x
        mesh.position.y = y
        mesh.position.z = z
        mesh.scale.set(
            Math.max(0.2, Math.random()), 
            Math.max(0.2, Math.random()),
            Math.max(0.2, Math.random()),
            )
        meshes.push(mesh)
        scene.add(mesh)
    }

    /************************************
    ************** Camera ***************
    ************************************/
    const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height, 0.1, 100)
    camera.position.z = 3
    camera.lookAt(new THREE.Vector3())
    scene.add(camera)

    /************************************
    ************* Controls **************
    ************************************/

    const controls = new DragControls( meshes, camera, canvas );
    let currentMeshColour;
    controls.addEventListener( 'dragstart', e => {
        currentMeshColour = e.object.material.color.clone() // Clone the colour upon dragstart
      e.object.material.color.set( 0xaaaaaa )
    })
    controls.addEventListener( 'dragend', e => {
        // Revert back to the original colour
      e.object.material.color = currentMeshColour;
    } );

    /************************************
    ************* Renderer **************
    ************************************/
    const renderer = new THREE.WebGLRenderer({
        canvas: canvas
    })
    renderer.setSize(sizes.width, sizes.height)
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))

    /************************************
    ********** Functionality ************
    ************************************/
    // Animate
    const clock = new THREE.Clock()

    const tick = () => {
        const elapsedTime = clock.getElapsedTime()

        // Update controls - no need for the drag controls
        //controls.update()

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
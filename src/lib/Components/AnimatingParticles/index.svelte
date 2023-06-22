<script>
  import { onMount } from 'svelte'
  import * as THREE from 'three'
  import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
  import gsap from 'gsap'
  export let id = 'id'


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
    ************* Particles *************
    ************************************/

    // Textures
    const textureLoader = new THREE.TextureLoader()
    const particleTexture = textureLoader.load('./textures/particles/6.png') // 1, 2, 5, 6, 10

    // Material 
    const material = new THREE.PointsMaterial({
        color: '#8448C1',
        size: 0.8,
        sizeAttenuation: true,
        transparent: true,
        depthWrite: false,
        alphaMap: particleTexture,
        blending: THREE.AdditiveBlending,
    })

    // Geometry 
    const MAX_POINTS = 10000 

    // Create 3 geometries, where two of them are the same, but make sure you copy the positions array
    // This is so that we can have a 'starting' position for the particles
    // Randomly position the particles in each; can also have different num particles
    const geometry = new THREE.BufferGeometry()
    const geometry1 = new THREE.BufferGeometry()
    const positions1 = new Float32Array(MAX_POINTS * 3)
    for (let i = 0; i < MAX_POINTS; i++) {
        const i3 = i * 3
        positions1[i3 + 0] = (Math.random() - 0.5) * 10
        positions1[i3 + 1] = (Math.random() - 0.5) * 10
        positions1[i3 + 2] = (Math.random() - 0.5) * 10
    }
    geometry.setAttribute('position', new THREE.BufferAttribute(positions1.slice(), 3))
    geometry1.setAttribute('position', new THREE.BufferAttribute(positions1, 3))

    const geometry2 = new THREE.BufferGeometry()
    const positions2 = new Float32Array(MAX_POINTS * 3)
    for (let i = 0; i < MAX_POINTS; i++) {
        const i3 = i * 3
        positions2[i3 + 0] = (Math.random() - 0.5) * 30
        positions2[i3 + 1] = (Math.random() - 0.5) * 20
        positions2[i3 + 2] = (Math.random() - 0.5) * 20
    }
    geometry2.setAttribute('position', new THREE.BufferAttribute(positions2, 3))

    // Particles 
    const particles = new THREE.Points(geometry, material) // Starting particles; geometry is a copy of geometry1 for particles1
    const particles1 = new THREE.Points(geometry1, material)
    const particles2 = new THREE.Points(geometry2, material)
    scene.add(particles) // Only add the initial particles

    /************************************
    ************** Camera ***************
    ************************************/
    const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height, 0.1, 100)
    camera.position.z = 20
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
    renderer.render(scene, camera) // Make sure to render the first time before any animation


    /************************************
    ************* Animation *************
    ************************************/

    // GSAP Animation 
    const buttonParticles1 = document.querySelector('.particles-1')
    const buttonParticles2 = document.querySelector('.particles-2')

    buttonParticles1.addEventListener('click', () => {
        gsapAnimateFrom1to2()
    })
    buttonParticles2.addEventListener('click', () => {
        gsapAnimateFrom2to1()
    })

    const gsapAnimateFrom1to2 = () => {
        gsap.to(particles.geometry.attributes.position.array, {
            endArray: particles2.geometry.attributes.position.array,
            duration: 3,
            ease: 'power2',
            // Make sure to tell it to update if not using the tick function
            onUpdate: () => {
              particles.geometry.attributes.position.needsUpdate = true;
              camera.lookAt(particles.position);
              renderer.render(scene, camera);
            },
        })
    }

    const gsapAnimateFrom2to1 = () => {
        gsap.to(particles.geometry.attributes.position.array, {
            endArray: particles1.geometry.attributes.position.array,
            duration: 3,
            ease: 'power2',
            // Make sure to tell it to update if not using the tick function
            onUpdate: () => {
              particles.geometry.attributes.position.needsUpdate = true;
              // controls.update() -- only works while it's updating
              camera.lookAt(particles.position);
              renderer.render(scene, camera);
            },
        })
    }

    // Update the controls only 
    const tick = () => {
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
  <button class='particles-1'>from particles 1 to 2</button>
  <button class='particles-2'>from particles 2 to 1</button>
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
      color: rgb(255, 255, 255);
      cursor: pointer;
    }

    button:hover {
        background-color: #8448C1;
        color: white;
    }

    .particles-1 {
        left: 10px;
    }

    .particles-2 {
        left: 250px;
    }
</style>
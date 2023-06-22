<script>
  import global from './global.css'
  import _ from 'lodash'
  import StarterCube from './lib/Components/StarterCube/index.svelte'
  import AnimatingParticles from './lib/Components/AnimatingParticles/index.svelte'
  import AddingParticles from './lib/Components/AddingParticles/index.svelte'
  import ShapedParticles from './lib/Components/ShapedParticles/index.svelte'
  import Draggables from './lib/Components/Draggables/index.svelte'
  import Explorable from './lib/Components/Explorable/index.svelte'
  import ObjectLoad from './lib/Components/ObjectLoad/index.svelte'

  const components = [
    {
      name: 'ObjectLoad',
      component: ObjectLoad
    },
    {
      name: 'StarterCube',
      component: StarterCube
    },
    {
      name: 'ShapedParticles',
      component: ShapedParticles
    },
    {
      name: 'AnimatingParticles',
      component: AnimatingParticles
    },
    {
      name: 'AddingParticles',
      component: AddingParticles
    },
    {
      name: 'Draggables',
      component: Draggables
    },
    {
      name: 'Explorable',
      component: Explorable
    },
  ]

  let selected;
  $: selectedComponent =
    selected && _.find(components, (d) => d.name === selected).component;
  $: selectedName = selected && _.find(components, (d) => d.name === selected).name

</script>

<main>
  <h1>Fun with Three.js</h1>
</main>

<!-- <section>
  <ObjectLoad id='0' />
  <StarterCube id='1' />
  <ShapedParticles id='2' />
  <AnimatingParticles id='3' />
  <AddingParticles id='4' />
  <Draggables id='5' />
  <Explorable id='6' />
</section> -->

<section>
<div class="wrapper-select">
  <label for="component-select">Choose a component:</label>
  <select bind:value="{selected}" name="components" id="component-select">
    {#each components as component}
      <option value="{component.name}">{component.name}</option>
    {/each}
  </select>
</div>
<!-- Display just the selected component -->
<svelte:component this="{selectedComponent}" />
</section>

<style lang='scss'>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }
  main {
    margin: 100px auto;
  }

  section {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;

  }

  h1 {
    color: #ffffff;
    text-shadow: 0 0 10px #fff, 0 0 20px #fff, 0 0 30px #ff69b4, 0 0 40px hotpink, 0 0 50px hotpink, 0 0 60px hotpink, 0 0 70px hotpink;
    text-transform: uppercase;
    font-size: 4rem;
    font-weight: 400;
    line-height: 1.1;
    margin: 30px;
    text-align: center;
  }

  h2, p, select, option, label {
    color: #ffffff;
  }

  .wrapper-select select {
    margin-bottom: 3rem;
  }

  @media (min-width: 480px) {
    h1 {
      max-width: none;
    }
  }
</style>

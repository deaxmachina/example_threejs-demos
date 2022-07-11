<script>
  import global from './global.css'
  import StarterCube from './lib/Components/StarterCube/index.svelte'
  import AnimatingParticles from './lib/Components/AnimatingParticles/index.svelte'
  import AddingParticles from './lib/Components/AddingParticles/index.svelte'
  import ShapedParticles from './lib/Components/ShapedParticles/index.svelte'
  import Draggables from './lib/Components/Draggables/index.svelte'
  import Explorable from './lib/Components/Explorable/index.svelte'

</script>

<main>
  <h1>Fun with Three.js</h1>
</main>

<section>
  <!-- <TestPattern id='test' /> -->
  <StarterCube id='0' />
  <ShapedParticles id='1' />
  <AnimatingParticles id='2' />
  <AddingParticles id='3' />
  <Draggables id='4' />
  <Explorable id='5' />
</section>

<style>
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

  @media (min-width: 480px) {
    h1 {
      max-width: none;
    }
  }
</style>

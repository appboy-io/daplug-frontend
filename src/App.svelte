<script lang="ts">
  import About from './lib/About.svelte'
import Hero from './lib/Hero.svelte'
import Navigation from './lib/Navigation.svelte'
  import Pricing from './lib/Pricing.svelte'
  import { currentUser, pb } from './lib/pocketbase';
  import BusinessList from './lib/BusinessList.svelte'
</script>

<main>

  {#if $currentUser}
    <Navigation />
    <BusinessList />
    {:else}
    <div>
      <Navigation />
      <Hero />
      <About />
      <Pricing />
    </div>
  {/if}
  <!-- If they are not logged in, show the following nave-->
  
</main>

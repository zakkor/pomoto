<script>
  import { fade } from 'svelte/transition';
  import Pomoto from '$lib/Pomoto.svelte';

  let active;
  let hoverEl = null;
</script>

<div class="flex flex-1 border border-gray-400 rounded-xl w-full h-16 pl-24">
  {#each { length: 5 } as _, i}
    <article
      class="flex items-center justify-center bg-indigo-300 w-8 h-8 self-center rounded 2xl"
      style="margin-left: {10 * i}px;"
      on:mouseenter={event => {
        active = i;
        hoverEl = event.target;
      }}
    >
      {i}
    </article>
  {/each}
</div>

<Pomoto trigger="hover" referenceEl={hoverEl} class="singleton">
  <div
    class="bg-indigo-100 rounded-xl px-3 py-2"
    transition:fade={{ duration: 200 }}
  >
    Hi there, I'm number {active}
  </div>
</Pomoto>

<style>
  :global(.singleton) {
    transition-property: left;
    transition-duration: 400ms;
    will-change: left;
  }
</style>

<script>
  import { scale } from 'svelte/transition';
  import Pomoto from '$lib/Pomoto.svelte';

  export let content = '';
</script>

<Pomoto trigger="hover">
  <slot slot="reference" />

  <div class="flex w-auto px-2 py-1 bg-black rounded-lg" transition:scale|local={{ duration: 100 }}>
    <button tabindex={1} class="text-sm font-medium text-white">
      {content}
    </button>
  </div>
</Pomoto>

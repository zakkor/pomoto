<script>
  import Pomoto from '$lib/Pomoto.svelte';
</script>

<h1>Tooltip</h1>

<p>Something about tooltips.</p>
Prop showcase should be on index page instead

<h2>Props</h2>

<!-- TODO: Prop showcase should be on index page instead -->

<h3>Prop <code>placement</code></h3>
<div class="grid grid-cols-4">
  {#each ['top', 'bottom', 'left', 'right'] as placement}
    <Pomoto role="tooltip" mode="css" {placement} class="bg-black text-white text-sm font-medium rounded-lg py-1 px-2">
      <button slot="reference" class="bg-pink-100 rounded-xl px-4 py-3">
        placement="{placement}"
      </button>
      Hello world!
    </Pomoto>
  {/each}
</div>

<h3>Prop <code>offset</code></h3>
<div class="flex flex-col items-start space-y-8">
  {#each [[0, 0], [32, 0], [0, 32], [-32, 0], [0, -32], [32, 32], [-32, -32]] as offset}
    <Pomoto
      role="tooltip"
      mode="css"
      placement="top"
      {offset}
      class="bg-black text-white text-sm font-medium rounded-lg py-1 px-2"
    >
      <button slot="reference" class="bg-pink-100 rounded-xl px-4 py-3">
        offset="[{offset}]"
      </button>
      Hello world!
    </Pomoto>
  {/each}
</div>

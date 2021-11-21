<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  export let el;
  export let visible;

  export let role;
  export let modal;
  let className = '';
  export { className as class };
  export let prerender;
  export let style;
</script>

<article
  bind:this={el}
  {role}
  aria-modal={modal}
  class={className}
  class:hidden={prerender && !visible}
  {style}
  tabindex={-1}
  on:keyup={event => {
    if (event.key === 'Escape') dispatch('close');
  }}
>
  <slot />
</article>

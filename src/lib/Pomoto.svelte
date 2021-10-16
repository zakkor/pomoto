<script>
  import { onMount, onDestroy, createEventDispatcher, tick } from 'svelte';

  const dispatch = createEventDispatcher();

  // Local bindings
  let el;
  let markerEl;

  export let visible = false;
  export let a11y;
  console.log(a11y);
  export let prerender = false; // lazy?
  export let clickOutside = true;
  let className = '';
  export { className as class };

  export let top = null;
  export let left = null;
  export let referenceEl = null;
  function reposition() {
    const rect = referenceEl.getBoundingClientRect();
    top = rect.bottom;
    left = rect.left + rect.width / 2;
  }
  $: if (markerEl && !referenceEl && $$slots.reference) referenceEl = markerEl.previousElementSibling;
  $: if (referenceEl) reposition();
  export let style = 'left: 50%; top: 50%; transform: translate(-50%, -50%)';
  $: if (top != null && left != null) style = `top: ${top}px; left: ${left}px; transform: translate(-50%)`;

  // Trigger and event listeners
  // TODO: Think about the reference/trigger semantics. What happens when reference is passed, but not trigger, vice versa, both passed, or none passed.
  export let trigger = 'click';
  const triggerEvents = {
    click: [{ name: 'click', fn: toggle }],
    hover: [
      { name: 'mouseenter', fn: open },
      { name: 'mouseleave', fn: close },
    ],
    manual: [],
  };
  const events = triggerEvents[trigger];
  function addEventListeners() {
    removeEventListeners();
    events.forEach(({ name, fn }) => triggerEl.addEventListener(name, fn));
  }
  function removeEventListeners() {
    if (triggerEl) events.forEach(({ name, fn }) => triggerEl.removeEventListener(name, fn));
  }
  $: triggerEl = markerEl?.previousElementSibling;
  $: if (triggerEl) addEventListeners();
  onDestroy(removeEventListeners);

  function open() {
    if (referenceEl) reposition();
    visible = true;
  }
  function close() {
    visible = false;
  }
  function toggle(event) {
    if (event) event.stopPropagation();
    visible ? close() : open();
  }

  $: if (visible) {
    dispatch('open');
    tick().then(() => dispatch('afterOpen'));
  } else {
    dispatch('close');
    tick().then(() => dispatch('afterClose'));
  }

  onMount(() => {
    // TODO: Prevent body scroll if opened, even if SSR
    // document.body.style.position = 'fixed';
  });
</script>

<svelte:window
  on:click={event => {
    if (clickOutside && visible && !el.contains(event.target)) {
      close();
    }
  }}
/>

<slot name="trigger" />
<slot name="reference" />
<div data-pomoto-marker bind:this={markerEl} hidden />

{#if prerender ? true : visible}
  <slot name="background" />
  <article
    bind:this={el}
    role="dialog"
    aria-modal="true"
    class="absolute {className}"
    class:hidden={prerender && !visible}
    {style}
    tabindex={0}
    on:keyup={event => {
      console.log(event);
      if (event.key === 'Escape') close();
    }}
  >
    <slot />
  </article>
{/if}

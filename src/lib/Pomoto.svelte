<script>
  import { onMount, onDestroy, createEventDispatcher, tick } from 'svelte';

  const dispatch = createEventDispatcher();

  // TODO: Prevent body scrolling
  // TODO: Support CSS-only rendering inside Pomoto
  // TODO: Support focus triggering

  export let visible = false;

  // Accessibility:
  export let role = 'dialog';
  export let modal = false;
  export let focusFirstElement = modal;
  export let focusTrap = modal;
  export let label = null;
  export let description = null;
  // TODO: Aria attributes:
  // aria-label/aria-labelledby: Added on both trigger and content, for all types of components where it makes sense
  // aria-describedby: Added on trigger, for all types of components where it makes sense
  // aria-expanded: Added on trigger, for all types of components, not needed on modals.
  // aria-haspopup: Added on trigger, _only_ for role="menu"

  export let mode = 'js';

  // Triggers and event listeners:
  // TODO: Add support for triggers in CSS mode. Make `click` work in CSS mode.
  // Triggers will behave completely different depending on mode.
  export let trigger = mode === 'js' ? ['click'] : [];

  export let closeOnClickOutside = !modal;

  // If both a reference element and a trigger element are passed:
  // interacting with the trigger will open/close, and the popover will be positioned relative to the reference.
  // If only a reference element is passed and no trigger element is passed, the reference becomes the trigger (popover behaviour).
  // If only a trigger element is passed and no reference element is passed, the modal will be positioned relative to the page (modal behaviour).
  // For more advanced usage the reference element can also be passed in as a prop, though usually it would be passed through the slot.
  export let referenceEl = null;

  // Default modal positioning.
  export let style = modal ? 'left: 50%; top: 50%; transform: translate(-50%, -50%)' : '';

  // TODO: Render self ahead of time after initial load, when the page is idle?
  export let prerender = false;
  export let portal = modal;
  let className = '';
  export { className as class };

  // Local bindings
  let el = null;
  let markerEl = null;
  let top = null;
  let left = null;

  function reposition() {
    const rect = referenceEl.getBoundingClientRect();
    top = rect.bottom + window.scrollY;
    left = rect.left + rect.width / 2 + window.scrollX;
  }

  // Rendered element and any background will be portaled out to `#portal`, which prevents `z-index` and CSS transform issues.
  // Typically, only modals should portal out.
  $: if (portal && el) {
    const portal = document.getElementById('portal');
    if (el.previousElementSibling !== markerEl) {
      portal.appendChild(el.previousElementSibling);
    }
    portal.appendChild(el);
  }
  // We use a hidden marker element to grab a reference to the trigger/reference elements which
  // lets us attach/detach event listeners to them without wrapping them in an extra element (which can affect the style of the page).
  $: if (markerEl && !referenceEl && $$slots.reference) referenceEl = markerEl.previousElementSibling;
  // `referenceEl` prop could be changed at any time. Make sure we react to changes and keep the position up to date.
  $: if (referenceEl) reposition();
  // If a reference is passed and we have its top/left coords, use them to position the popover element.
  // TODO: top/bottom/left/right positions + offset.
  $: if (top != null && left != null) style = `top: ${top}px; left: ${left}px; transform: translate(-50%)`;

  const triggerEvents = {
    click: [{ name: 'click', fn: toggle }],
    hover: [
      { name: 'mouseenter', fn: open },
      { name: 'mouseleave', fn: close },
    ],
    // TODO: Focus
    manual: [],
  };
  const events = trigger.flatMap(t => triggerEvents[t]).filter(Boolean);
  function addEventListeners() {
    removeEventListeners();
    events.forEach(({ name, fn }) => triggerEl.addEventListener(name, fn));
  }
  function removeEventListeners() {
    if (triggerEl) events.forEach(({ name, fn }) => triggerEl.removeEventListener(name, fn));
  }

  // Determine trigger element: if trigger exists, get that. Otherwise try getting the reference element.
  let triggerEl = null;
  $: if (markerEl) triggerEl = markerEl.previousElementSibling;
  // Set up event listeners
  $: if (triggerEl && mode === 'js') addEventListeners();
  // Cleanup event listeners
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

  // Modal accessibility: focus first element and trap focus inside modal.
  const focusableSel = 'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])';

  function focusFirst() {
    // Focus first focusable element, or dialog element itself.
    const focusable = el.querySelectorAll(focusableSel);
    const focusEl = focusable[0] || el;
    focusEl.focus();
  }

  function attachFocusTrap() {
    const focusable = el.querySelectorAll(focusableSel);
    if (focusable.length === 0) return;

    // If focus were to leave the first element, move focus to last element instead.
    const onBlur = event => {
      event.preventDefault();
      if (event.relatedTarget !== focusable[1]) {
        focusable[focusable.length - 1].focus();
      }
    };
    focusable[0].addEventListener('blur', onBlur);

    // If focus were to leave the last element, move focus to first element instead.
    // The "blur" event for the last element is a bit special: if there is nothing else left to focus on the page,
    // the focus moves to the browser address bar, which cannot be prevented.
    // To get around this, instead of using the "blur" event, we hijack the Tab key in the "keydown" event instead.
    const onTab = event => {
      if (event.key !== 'Tab' || event.shiftKey === true) return;
      event.preventDefault();
      focusable[0].focus();
    };
    focusable[focusable.length - 1].addEventListener('keydown', onTab);

    return () => {
      focusable[0].removeEventListener('blur', onBlur);
      focusable[focusable.length - 1].removeEventListener('keydown', onTab);
    };
  }

  // Dispatch events: open, afterOpen, close, afterClose
  $: if (visible) {
    dispatch('open');
    tick().then(() => {
      dispatch('afterOpen');
    });
  } else {
    // TODO: Why is the close event firing on initial render?
    dispatch('close');
    tick().then(() => {
      dispatch('afterClose');
    });
  }
  // Use separate reactive statements for focus trap attach and cleanup to prevent infinite updates.
  let removeFocusTrap = null;
  $: if (visible) {
    tick().then(() => {
      if (focusTrap) removeFocusTrap = attachFocusTrap();
      if (focusFirstElement) focusFirst();
    });
  }
  $: if (!visible) {
    // Cleanup focus trap handlers.
    removeFocusTrap?.();
    // When modal closes, return focus to the trigger element.
    triggerEl?.focus();
  }

  onMount(() => {
    // TODO: Prevent body scroll if opened, even if SSR
    // document.body.style.position = 'fixed';
  });
</script>

<svelte:window
  on:click={event => {
    if (closeOnClickOutside && visible && !el.contains(event.target)) {
      close();
    }
  }}
/>

{#if mode === 'css'}
  <button tabindex={0} aria-label={label} class="group relative">
    <slot name="reference" />
    <article
      bind:this={el}
      {role}
      class="invisible opacity-0 group-hover:visible group-hover:opacity-100 group-focus:visible group-focus:opacity-100 absolute bottom-full left-1/2 -translate-x-1/2 w-max transition-opacity {className}"
      {style}
      tabindex={-1}
    >
      <slot />
    </article>
  </button>
{:else}
  <slot name="reference" />
  <slot name="trigger" />
  <div data-pomoto-marker bind:this={markerEl} hidden aria-hidden="true" />

  {#if prerender ? true : visible}
    <slot name="background" />
    <article
      bind:this={el}
      {role}
      aria-modal={modal}
      class="absolute {className}"
      class:hidden={prerender && !visible}
      {style}
      tabindex={-1}
      on:keyup={event => {
        if (event.key === 'Escape') close();
      }}
    >
      <slot />
    </article>
  {/if}
{/if}

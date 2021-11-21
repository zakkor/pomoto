<script>
  import { onMount, onDestroy, createEventDispatcher, tick } from 'svelte';
  import Element from './Element.svelte';

  const dispatch = createEventDispatcher();

  export let visible = false;
  export let mode = 'js';

  // Triggers and event listeners:
  // In 'css' mode the only triggers that exist (and are forced enabled) are 'focus' + 'hover'.
  // TODO: Think of a way to make triggers configurable in CSS mode too and how to make them work together.
  export let trigger = mode === 'js' ? ['click'] : [];

  export let zIndex = 'z-10';

  // Accessibility:
  export let role = 'dialog';
  export let modal = false;
  export let focusFirstElement = modal;
  export let focusTrap = modal;
  export let returnFocus = modal;
  export let ariaLabel = null;
  export let ariaDescribedBy = null;
  export let ariaHasPopup = role === 'menu';

  export let closeOnClickOutside = !modal;

  // If both a reference element and a trigger element are passed:
  // interacting with the trigger will open/close, and the popover will be positioned relative to the reference.
  // If only a reference element is passed and no trigger element is passed, the reference becomes the trigger (popover behaviour).
  // If only a trigger element is passed and no reference element is passed, the modal will be positioned relative to the page (modal behaviour).
  // For more advanced usage the reference element can also be passed in as a prop, though usually it would be passed through the slot.
  export let referenceEl = null;

  // Default modal positioning.
  // export let style = $$slots.reference ? 'margin: auto' : 'position: absolute';

  // TODO: Render self ahead of time after initial load, when the page is idle?
  export let prerender = false;
  export let portal = modal;
  let className = '';
  export { className as class };

  /** @type {'top' | 'right' | 'bottom' | 'left'} */
  export let placement = 'top';
  export let offset = [0, 0]; // [x, y]

  // Local bindings
  let el = null;
  let markerEl = null;
  let top = null;
  let left = null;
  let translateX = null;
  let translateY = null;

  function repositionAbsolute() {
    const rect = referenceEl.getBoundingClientRect();
    const [offsetX, offsetY] = offset;
    switch (placement) {
      case 'top':
        left = rect.left + rect.width / 2 + window.scrollX + offsetX;
        translateX = '-50%';
        top = rect.top + window.scrollY + offsetY;
        translateY = '-100%';
        break;
      case 'bottom':
        top = rect.bottom + window.scrollY;
        left = rect.left + rect.width / 2 + window.scrollX;
        break;
      default:
        break;
    }
  }

  function repositionRelative() {
    const [offsetX, offsetY] = offset;
    switch (placement) {
      case 'top':
        left = `calc(50% + ${offsetX}px)`;
        translateX = '-50%';
        top = `${offsetY}px`;
        translateY = '-100%';
        break;
      case 'bottom':
        left = `calc(50% + ${offsetX}px)`;
        translateX = '-50%';
        top = `calc(100% + ${offsetY}px)`;
        break;
      case 'left':
        left = `${offsetX}px`;
        translateX = '-100%';
        top = `calc(50% + ${offsetY}px)`;
        translateY = '-50%';
        break;
      case 'right':
        left = `calc(100% + ${offsetX}px)`;
        top = `calc(50% + ${offsetY}px)`;
        translateY = '-50%';
        break;
    }
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
  $: if (mode === 'js' && markerEl && !referenceEl && $$slots.reference) referenceEl = markerEl.previousElementSibling;
  // `referenceEl` prop could be changed at any time. Make sure we react to changes and keep the position up to date.
  $: if (mode === 'js' && referenceEl) repositionAbsolute();
  $: if (mode === 'css' && $$slots.reference) repositionRelative();
  // If a reference is passed and we have its top/left coords, use them to position the popover element.
  let positionStyle = '';
  $: if (top != null && left != null) {
    positionStyle = `position: absolute; top: ${top}${typeof top === 'number' ? 'px' : ''}; left: ${left}${
      typeof left === 'number' ? 'px' : ''
    }; transform: translate(${translateX || 0}, ${translateY || 0})`;
  } else {
    positionStyle = `margin: auto`;
  }

  const triggerEvents = {
    click: [{ name: 'click', fn: toggle }],
    hover: [
      { name: 'mouseenter', fn: open },
      { name: 'mouseleave', fn: close },
    ],
    focus: [
      { name: 'focus', fn: open },
      { name: 'blur', fn: close },
    ],
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
    if (mode === 'js' && referenceEl) repositionAbsolute();
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
    if (returnFocus) triggerEl?.focus();
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
  <slot name="trigger" />

  {#if $$slots.reference}
    <div class="relative">
      <slot name="reference" />
      <Element
        bind:el
        {role}
        {modal}
        class="{className} {zIndex} invisible opacity-0 absolute w-max transition-opacity"
        style={positionStyle}
        on:close={close}
      >
        <slot />
      </Element>
    </div>
  {/if}
  {#if $$slots.trigger}
    <Element
      bind:el
      {role}
      {modal}
      class="{className} {zIndex} invisible opacity-0 absolute w-max transition-opacity"
      style={positionStyle}
      on:close={close}
    >
      <slot />
    </Element>
  {/if}
{:else}
  <slot name="reference" />
  <slot name="trigger" />
  <div data-pomoto-marker bind:this={markerEl} hidden aria-hidden="true" />

  {#if prerender ? true : visible}
    <slot name="bg" />
    <Element bind:el bind:visible {role} {modal} class="{className} {zIndex}" {prerender} style={positionStyle} on:close={close}>
      <slot />
    </Element>
  {/if}
{/if}

<style global>
  [slot='reference'] + article:hover,
  [slot='reference'] + article:focus,
  [slot='reference']:hover + article,
  [slot='reference']:focus + article {
    @apply visible opacity-100;
  }
  [slot='trigger'] + article:hover,
  [slot='trigger'] + article:focus,
  [slot='trigger']:hover + article,
  [slot='trigger']:focus + article {
    @apply visible opacity-100;
  }
</style>

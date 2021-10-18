<script>
  let className = '';
  export { className as class };
  export let ariaLabel = null;
</script>

<span role="button" tabindex={0} aria-label={ariaLabel}>
  <slot name="trigger" />
  <div role="tooltip" class={className}>
    <slot />
  </div>
</span>

<style>
  [role='button'] {
    position: relative;
  }
  [role='tooltip'] {
    width: max-content;
    opacity: 0;
    visibility: hidden;
    position: absolute;
    left: 0;
    top: 100%;
  }
  [role='button']:focus > [role='tooltip'],
  [role='button']:hover > [role='tooltip'] {
    opacity: 1;
    visibility: visible;
  }
</style>

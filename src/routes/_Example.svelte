<script>
  export let title = '';
</script>

<li class="flex flex-col items-start">
  <h1 class="text-2xl font-medium mb-3">
    {title}
  </h1>
  <slot />
</li>

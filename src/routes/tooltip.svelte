<script>
  import Tooltip from '$lib/Tooltip.svelte';
</script>

<svelte:head>
  <link href="https://unpkg.com/tailwindcss@2.2.10/dist/tailwind.min.css" rel="stylesheet" />
</svelte:head>

<div class="flex flex-col items-center space-y-4">
  <h2 class="text-xl font-semibold">Press <kbd>Tab</kbd> to cycle between focusable elements</h2>

  <button> Focusable element 1 </button>

  <Tooltip class="bg-black text-white font-semibold px-5 py-3 rounded-2xl">
    <svg slot="trigger" width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
      <g opacity=".64">
        <path
          d="M7.595 9.763h1.27v-.362c.033-.635.577-1.1 1.11-1.61.51-.498.997-1.02.997-1.824 0-1.1-.759-1.984-2.38-1.984-1.518 0-2.572.93-2.652 2.414h1.315c.08-.782.567-1.28 1.27-1.28.69 0 1.008.374 1.008.952 0 .487-.374.861-.793 1.28-.544.533-1.145 1.088-1.145 1.984v.43Zm.623 2.516c.499 0 .907-.385.907-.872a.89.89 0 0 0-.907-.884c-.51 0-.93.385-.93.884 0 .487.42.872.93.872Z"
          fill="#77748C"
        />
        <path
          d="M.567 3.853A3.287 3.287 0 0 1 3.853.567h8.294a3.287 3.287 0 0 1 3.286 3.286v8.294a3.287 3.287 0 0 1-3.286 3.286H3.853a3.287 3.287 0 0 1-3.286-3.286V3.853Z"
          stroke="#77748C"
          stroke-width="1.133"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </g>
    </svg>
    Hello there!
  </Tooltip>

  <button> Focusable element 2 </button>
</div>

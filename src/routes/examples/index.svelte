<script>
  import { scale, fade } from 'svelte/transition';
  import { sineInOut as easing } from 'svelte/easing';
  import Pomoto from '$lib/Pomoto.svelte';

  import Example from '../_Example.svelte';
  import Singleton from '../_Singleton.svelte';
  import Button from '../_ButtonExample.svelte';

  let mounted = true;

  let inputEl;
</script>

<svelte:head>
  <link href="https://unpkg.com/tailwindcss@2.2.10/dist/tailwind.min.css" rel="stylesheet" />
</svelte:head>

<button on:click={() => (mounted = !mounted)} class="mb-8">
  {mounted ? 'Unmount' : 'Mount'} Pomoto
</button>

{#if mounted}
  <div class="flex flex-col items-start space-y-16">
    <Example title="Dropdown (JS)">
      <Pomoto>
        <Button slot="reference" class="bg-blue-200 rounded-md p-3">Click me to open</Button>
        <div class="flex w-auto bg-gray-700 rounded-lg px-6 py-4" transition:fade|local={{ duration: 100 }}>
          <p class="text-sm font-medium text-white">Hello world</p>
        </div>
      </Pomoto>
    </Example>

    <Example title="Dropdown (CSS)">
      <Pomoto mode="css">
        <Button slot="reference" class="bg-blue-200 rounded-md p-3">Hover over me</Button>
        <div class="flex w-auto bg-gray-700 rounded-lg px-6 py-4" transition:fade|local={{ duration: 100 }}>
          <p class="text-sm font-medium text-white">Hello world</p>
        </div>
      </Pomoto>
    </Example>

    <Example title="Tooltip (JS): trigger={['click', 'hover']}">
      <Pomoto role="tooltip" trigger={['click', 'hover']}>
        <button slot="reference">Hover over me</button>
        <div
          class="flex w-auto text-sm font-medium text-white bg-black rounded-lg px-2 py-1"
          transition:scale|local={{ duration: 100 }}
        >
          Hello world!
        </div>
      </Pomoto>
    </Example>

    <Example title="Tooltip (JS): trigger={['click', 'hover', 'focus']}">
      <Pomoto role="tooltip" trigger={['click', 'hover', 'focus']}>
        <button slot="reference">Hover over me</button>
        <div
          class="flex w-auto text-sm font-medium text-white bg-black rounded-lg px-2 py-1"
          transition:scale|local={{ duration: 100 }}
        >
          Hello world!
        </div>
      </Pomoto>
    </Example>

    <Example title="Tooltip (CSS)">
      <Pomoto role="tooltip" mode="css" class="bg-black text-white text-sm font-medium rounded-lg py-1 px-2">
        <span slot="reference">Hover over me</span>
        Hello world!
      </Pomoto>
    </Example>

    <Example title="Modal (JS)">
      <Pomoto modal closeOnClickOutside>
        <Button slot="trigger" class="bg-blue-200 rounded-md p-3">Click me to open</Button>
        <div
          class="flex flex-col bg-gray-700 rounded-lg px-10 py-8"
          style="width: 600px"
          transition:scale|local={{ start: 0.95, duration: 200 }}
        >
          <p class="text-2xl font-medium text-white mb-4">Hello world</p>
          <p class="text-base text-white">
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam a euismod velit. Mauris mattis tempor purus
            blandit tempor. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos.
            Quisque maximus non massa sit amet faucibus. Cras sed consectetur metus. In hac habitasse platea dictumst.
            Nulla urna erat, elementum a tortor at, varius tristique felis. Cras in lacus eget orci porta cursus eu a
            nisi. Proin tortor neque, sollicitudin nec gravida vitae, finibus a eros. Integer faucibus elit et mauris
            efficitur condimentum. Cras ut diam magna. Etiam augue mi, faucibus nec odio nec, eleifend venenatis dolor.
            Integer dolor diam, lobortis eget fringilla eget, ullamcorper et est. Sed non est id leo fringilla
            ullamcorper ut id arcu. Duis vitae dui vitae turpis pretium ullamcorper.
          </p>
        </div>
      </Pomoto>
    </Example>

    <Example title="Modal (CSS)">
      <Pomoto mode="css" closeOnClickOutside>
        <Button slot="trigger" class="bg-blue-200 rounded-md p-3">Click me to open</Button>
        <div
          class="flex flex-col bg-gray-700 rounded-lg px-10 py-8"
          style="width: 600px"
          transition:scale|local={{ start: 0.95, duration: 200 }}
        >
          <p class="text-2xl font-medium text-white mb-4">Hello world</p>
          <p class="text-base text-white">
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam a euismod velit. Mauris mattis tempor purus
            blandit tempor. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos.
            Quisque maximus non massa sit amet faucibus. Cras sed consectetur metus. In hac habitasse platea dictumst.
            Nulla urna erat, elementum a tortor at, varius tristique felis. Cras in lacus eget orci porta cursus eu a
            nisi. Proin tortor neque, sollicitudin nec gravida vitae, finibus a eros. Integer faucibus elit et mauris
            efficitur condimentum. Cras ut diam magna. Etiam augue mi, faucibus nec odio nec, eleifend venenatis dolor.
            Integer dolor diam, lobortis eget fringilla eget, ullamcorper et est. Sed non est id leo fringilla
            ullamcorper ut id arcu. Duis vitae dui vitae turpis pretium ullamcorper.
          </p>
        </div>
      </Pomoto>
    </Example>

    <Example title="Modal with background">
      <Pomoto modal>
        <Button slot="trigger" class="bg-blue-200 rounded-md p-3">Click me to open</Button>
        <div
          slot="bg"
          class="absolute inset-0 bg-black bg-opacity-50 backdrop-filter backdrop-blur-md"
          transition:fade|local={{ duration: 250, easing }}
        />
        <div
          class="flex flex-col bg-gray-700 rounded-lg px-10 py-8"
          style="width: 600px"
          transition:scale|local={{ start: 0.95, duration: 200 }}
        >
          <p class="text-2xl font-medium text-white mb-4">Hello world</p>
          <p class="text-base text-white">
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam a euismod velit. Mauris mattis tempor purus
            blandit tempor. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos.
            Quisque maximus non massa sit amet faucibus. Cras sed consectetur metus. In hac habitasse platea dictumst.
            Nulla urna erat, elementum a tortor at, varius tristique felis. Cras in lacus eget orci porta cursus eu a
            nisi. Proin tortor neque, sollicitudin nec gravida vitae, finibus a eros. Integer faucibus elit et mauris
            efficitur condimentum. Cras ut diam magna. Etiam augue mi, faucibus nec odio nec, eleifend venenatis dolor.
            Integer dolor diam, lobortis eget fringilla eget, ullamcorper et est. Sed non est id leo fringilla
            ullamcorper ut id arcu. Duis vitae dui vitae turpis pretium ullamcorper.
          </p>
        </div>
      </Pomoto>
    </Example>

    <Example title="Modal events: Perform action on elements after they are mounted">
      <Pomoto
        modal
        on:open={() => console.log('open')}
        on:afterOpen={() => {
          console.log('afterOpen');
          inputEl.value = 'Hello there';
        }}
        on:close={() => console.log('close')}
        on:afterClose={() => console.log('afterClose')}
      >
        <Button slot="trigger" class="bg-blue-200 rounded-md p-3">Click me to open</Button>
        <div
          class="flex flex-col bg-gray-700 rounded-lg px-10 py-8"
          style="width: 600px"
          transition:scale|local={{ start: 0.95, duration: 200 }}
        >
          <input
            bind:this={inputEl}
            type="text"
            class="w-full p-3"
            placeholder="Input should be automatically focused on modal open"
          />
        </div>
      </Pomoto>
    </Example>

    <Example title="Modal a11y: First element should have focus">
      <Pomoto modal>
        <Button slot="trigger" class="bg-blue-200 rounded-md p-3">Click me to open</Button>
        <div
          class="flex flex-col bg-gray-700 rounded-lg px-10 py-8"
          style="width: 600px"
          transition:scale|local={{ start: 0.95, duration: 200 }}
        >
          <input type="text" class="w-full p-3" placeholder="Input should be automatically focused on modal open" />
        </div>
      </Pomoto>
    </Example>

    <Example title="Modal a11y: Escape to close">
      <Pomoto modal>
        <Button slot="trigger" class="bg-blue-200 rounded-md p-3">Click me to open</Button>
        <div
          class="flex flex-col bg-gray-700 text-white rounded-lg px-10 py-8"
          style="width: 600px"
          transition:scale|local={{ start: 0.95, duration: 200 }}
        >
          <h2>Press escape to close modal.</h2>
        </div>
      </Pomoto>
    </Example>

    <Example title="Modal a11y: Focus trap">
      <Pomoto modal>
        <Button slot="trigger" class="bg-blue-200 rounded-md p-3">Click me to open</Button>
        <div
          class="flex flex-col bg-gray-700 rounded-lg px-10 py-8 space-y-4"
          style="width: 600px"
          transition:scale|local={{ start: 0.95, duration: 200 }}
        >
          <input type="text" class="w-full p-3" placeholder="Tab through inputs" />
          <input type="text" class="w-full p-3" placeholder="Tab through inputs" />
          <input type="text" class="w-full p-3" placeholder="Tab through inputs" />
          <input type="text" class="w-full p-3" placeholder="Tab through inputs" />
        </div>
      </Pomoto>
    </Example>

    <Example title="Singleton">
      <Singleton />
    </Example>
  </div>
{/if}

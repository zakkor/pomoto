<script>
  import { scale } from 'svelte/transition';
  import Pomoto from '$lib/Pomoto.svelte';
</script>

<h1>Pomoto</h1>

<p class="text-lg">
  Pomoto (<strong>po</strong>pover, <strong>mo</strong>dal, <strong>to</strong>oltip) is a zero-dependency all-in-one
  solution you can use to implement any kind of popover-like content, with a focus on <em>performance</em>,
  <em>developer ergonomics</em>, and <em>accessibility</em>.
</p>

<p class="text-lg">
  Check out <a href="/examples">the examples page</a> for some examples of how to implement commonly used UI components.
</p>

<h2>Approach</h2>

<pre>
  <code>
    {`<Pomoto>
  <button slot="trigger">Click me</button>
  <div>
    Content goes here.
  </div>
</Pomoto>`}
  </code>
</pre>

<div class="bg-gray-50 border h-[600px] w-full px-8 pt-[200px]">
  <p class="w-[390px]">
    Bill Murray <em>(born September 21, 1950)</em> is an American actor, comedian, and writer. Known for his <Pomoto
      role="tooltip"
      placement="top"
      offset={[0, -8]}
    >
      <button slot="reference" class="font-semibold !text-pink-500 underline"> deadpan delivery.</button>
      <div
        class="bg-white w-[400px] border rounded-xl shadow-lg px-6 py-4"
        transition:scale|local={{ start: 0.95, duration: 200 }}
      >
        <strong>Deadpan</strong>, dry humour, or dry-wit humour is the deliberate display of emotional neutrality or no
        emotion, commonly as a form of comedic delivery to contrast with the ridiculousness or absurdity of the subject
        matter.
      </div>
    </Pomoto>
  </p>
</div>

<h2>Rendering modes</h2>

<h2>Props</h2>

<h2>Events</h2>

<h2>Slots</h2>

<ul>
  <li>Trigger: aslkdjaksdj asda</li>
  <li>Reference: asjdklajs dalskjd</li>
  <li>Background</li>
</ul>

<h2>Accessibility</h2>

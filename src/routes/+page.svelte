<script lang="ts">
  import FlipClock from '$lib/FlipClock.svelte';
  import '../global.css';

  let date = new Date();
  date.setHours(10);
  date.setMinutes(14);
  date.setSeconds(2);

  setInterval(() => date.setMinutes(Math.floor(Math.random() * 60)), 1000);
</script>

<h1>svelte-flip-clock demo</h1>
<div class="demo">
  <FlipClock bind:date size={2} />
</div>
<a href="https://github.com/ronanru/svelte-flip-clock">Documentation on GitHub</a>

<style>
  h1 {
    margin: 0;
  }
  .demo {
    display: grid;
    place-items: center;
  }
  a {
    display: block;
    color: black;
    font-size: 1.1em;
  }
</style>

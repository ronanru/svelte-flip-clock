# svelte-flip-clock

A flip clock component for Svelte

Demo: [svelte-flip-clock.ronanru.dev](https://svelte-flip-clock.ronanru.dev)

# Installation

```
npm install svelte-flip-clock
```

# Usage

All props are optional

```html
<script>
  import FlipClock from 'svelte-flip-clock';
</script>

<FlipClock
  showHours={true}
  showMinutes={true}
  showSeconds={true}
  size={2}
  textColor="white"
  backgroundColor="#383838"
/>
```

By default clock will get the time from `new Date()`, but you can supply your own date

```html
<script>
  import FlipClock from 'svelte-flip-clock';

  let myCustomDate = new Date();
  myCustomDate.setHours(4)
  myCustomDate.setMinutes(20)
  myCustomDate.setSeconds(69)
</script>

<FlipClock bind:date={myCustomDate} />
```

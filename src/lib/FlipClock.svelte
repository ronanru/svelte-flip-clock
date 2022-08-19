<script lang="ts">
  import { onMount, onDestroy } from 'svelte';

  export let showHours = true,
    showMinutes = true,
    showSeconds = true,
    size = 1,
    textColor = 'white',
    backgroundColor = '#383838',
    date: Date | null = null;

  $: segmentsCount = Number(showHours) + Number(showMinutes) + Number(showSeconds);

  let interval: ReturnType<typeof setInterval>,
    display = new Array(Number(showHours) + Number(showMinutes) + Number(showSeconds)).fill({
      top: '00',
      bottom: '00',
      transition: false
    });

  onMount(
    () =>
      (interval = setInterval(() => {
        const myDate = date || new Date();
        const newData: string[] = [];
        if (showHours) newData.push(myDate.getHours().toString().padStart(2, '0'));
        if (showMinutes) newData.push(myDate.getMinutes().toString().padStart(2, '0'));
        if (showSeconds) newData.push(myDate.getSeconds().toString().padStart(2, '0'));
        display = display.map(({ bottom }, i) => ({
          top: newData[i],
          bottom,
          transition: newData[i] !== bottom
        }));
        setTimeout(
          () =>
            (display = display.map(({ top }, i) => ({
              bottom: newData[i],
              top,
              transition: false
            }))),
          500
        );
      }, 1000))
  );
  onDestroy(() => interval && clearInterval(interval));
</script>

<div
  class="clock"
  style:width="calc((2ch + 0.5em) * {segmentsCount} + 0.1em * {segmentsCount - 1})"
  style:font-size="{size * 2}em"
  style:--flip-clock-text-color={textColor}
  style:--flip-clock-background-color={backgroundColor}>
  <div class="overlay">
    {#each display as segment}
      <div class="segment">
        <p class="top" class:transition={segment.transition}>
          <span>
            {segment.bottom}
          </span>
        </p>
        <p class="bottom" class:transition={segment.transition}>
          <span>
            {segment.top}
          </span>
        </p>
      </div>
    {/each}
  </div>
  <div class="base">
    {#each display as segment}
      <div class="segment">
        <p class="top">
          {segment.top}
        </p>
        <p class="bottom">
          {segment.bottom}
        </p>
      </div>
    {/each}
  </div>
</div>

<style lang="scss">
  .clock {
    position: relative;
    height: 1.51em;
  }
  .base,
  .overlay {
    display: flex;
    gap: 0.1em;
  }
  .overlay {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 10;
    .bottom {
      transform: scaleY(0);
    }
  }
  .base {
    p {
      border-radius: 0.25em;
    }
  }
  .segment {
    position: relative;
    color: var(--flip-clock-text-color);
    width: 3ch;
    height: 1.51em;
    width: calc(2ch + 0.5em);
    p {
      background: var(--flip-clock-background-color);
      padding: 0.125em 0.25em;
      margin: 0;
      width: 2ch;
      text-align: center;
    }
    .top {
      border-radius: 0.25em 0.25em 0 0;
      position: absolute;
      top: 0;
      left: 0;
      margin: 0;
      clip-path: inset(0px 0px 50% 0px);
      &.transition {
        transition: transform 0.25s ease-in;
        transform: scaleY(0);
      }
    }
    .bottom {
      border-radius: 0 0 0.25em 0.25em;
      clip-path: inset(50% 0px 0px 0px);
      top: 0.01em;
      left: 0;
      position: absolute;
      margin: 0;
      &.transition {
        transition: transform 0.25s 0.25s ease-out;
        transform: scale(100%);
      }
    }
  }
</style>

<script>
  import { fade } from "svelte/transition";
  export let dateScale;
  export let valueScale;
  export let width;
  export let dateTicks;
  export let mini = false;
  export let show = false;
  export let type;
  export let valueTicks = [];
</script>

{#if show}
  <g class="axes" transition:fade>
    <text
      class="label"
      transform="translate(15 {valueScale.range()[0]}) rotate(-90)">Time</text
    >
    <g class="axis-date" transform="translate(0 {valueScale.range()[0]})">
      {#each dateTicks as tick}
      {#if mini}
        <g transform="translate({dateScale(tick)} 0)">
        <text x="0" y="0"
          >{new Date(tick * 1000).toLocaleDateString(undefined, {month: "short", day: "numeric"} )}</text
        >
        <line x1="0" y1="-12" x2="0" y2="-21" />
        </g>       
        {:else}
          <g transform="translate({dateScale(tick)} 0)">
          <text x="0" y="0"
            >{new Date(tick * 1000).toLocaleDateString("de-De")}</text
          >
          <line x1="0" y1="-12" x2="0" y2="-21" />
          </g>
        {/if}
      {/each}
    </g>
    <g class="axis-value">
      <text
        class="label"
        transform="translate(15 {valueScale(valueTicks[5])}) rotate(-90)"
        >{type}</text
      >
      {#each valueTicks.slice(1) as tick}
        <g transform="translate(0 {valueScale(tick)})">
          <line
            class:faint={tick !== valueTicks[11]}
            x1="55"
            y1="0"
            x2={width}
            y2="0"
          />
          <text x="25" y="4">{tick.toPrecision(3)}</text>
        </g>
      {/each}
    </g>
  </g>
{/if}

<style>
  line {
    stroke: var(--red);
    stroke-width: 0.2vmin;
    stroke-opacity: 0.1;
  }
  .axis-value line.faint {
    stroke-opacity: 0.05;
  }

  text {
    font-family: "Source Sans Pro", sans-serif;
    font-size: 0.8rem;
    fill: var(--purple);
    fill-opacity: 0.4;
  }
  .axis-date text {
    text-anchor: middle;
  }
  .axis-value text {
    text-anchor: start;
  }
  text.label {
    text-anchor: middle;
  }
</style>

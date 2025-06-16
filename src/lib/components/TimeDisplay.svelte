<script lang="ts">
  import { onMount } from 'svelte';

  const divider = "|";

  function toDecimalTime(date: Date): number {
    const totalSecondsInDay = 24 * 60 * 60;
    const totalDecimalSecondsInDay = 10 * 100 * 100;
    const currentSeconds = (date.getHours() * 3600 + date.getMinutes() * 60 + date.getSeconds()) + date.getMilliseconds() / 1000;
    const decimalSeconds = (currentSeconds / totalSecondsInDay) * totalDecimalSecondsInDay;
    return decimalSeconds;
  }

  function formatDecimalTime(decimalSeconds: number): string {
    const decimalHours = Math.floor(decimalSeconds / (100 * 100));
    const decimalMinutes = Math.floor((decimalSeconds % (100 * 100)) / 100);
    const decimalSecondsRemaining = Math.floor(decimalSeconds % 100);
    return `${decimalHours}${divider}${decimalMinutes.toString().padStart(2, '0')}${divider}${decimalSecondsRemaining.toString().padStart(2, '0')}`;
  }

  let decimalSeconds = 0;
  $: decimalTime = formatDecimalTime(decimalSeconds);
  $: indicatorPosition = (decimalSeconds / (10 * 100 * 100)) * 100;

  let rafId: number;

  onMount(() => {
    const updateLoop = () => {
      decimalSeconds = toDecimalTime(new Date());
      rafId = requestAnimationFrame(updateLoop);
    };
    updateLoop();

    return () => {
      cancelAnimationFrame(rafId);
    };
  });

  // time points
  let timePoints: { name: string, time: string, decimaltime: string }[] = [];

  function addTimePoint() {
    const now = new Date();
    const hours = now.getHours().toString().padStart(2, '0');
    const minutes = now.getMinutes().toString().padStart(2, '0');
    const decimalTime = formatDecimalTime(toDecimalTime(now));
    timePoints = [...timePoints, { name: "Time Point", time: `${hours}:${minutes}`, decimaltime: decimalTime }];
  }

</script>

<h2>Decimal Time</h2>
<p>{decimalTime}</p>

<div class="time-bar">
  <div class="indicator" style="left: {indicatorPosition}%"></div>
  <div class="bar">
    {#each timePoints as point}
      <div class="point" style="left: {(point.decimaltime)}%"></div>
    {/each}
    </div>
</div>

<button on:click={addTimePoint}>Add Time Point</button>

<div class="time-points">
    {#each timePoints as point}
      <div class="time-point">
        <span>{point.name}</span>
        <span>{point.time}</span>
        <span>{point.decimaltime}</span>
      </div>
    {/each}
</div>

<style lang="postcss">
  h2 {
    font-size: 1.5rem;
    margin-bottom: -1rem;
    color: #007BFF;
  }

  p {
    font-size: 2.5rem;
    font-weight: bold;
    margin-bottom: 1rem;
  }

  .time-bar {
    width: 100%;
    height: 20px;
    background: linear-gradient(
      to right,
      #001f3f 0%,
      #00449e 10%,
      #ffd700 25%,
      #ffeb3b 50%,
      #ffd700 75%,
      #00449e 90%,
      #001f3f 100%
    );
    position: relative;
    border-radius: 10px;
    overflow: hidden;
  }

  .indicator {
    width: 4px;
    height: 100%;
    background-color: #ff4136;
    position: absolute;
    top: 0;
    transition: left 0.1s linear;
  }

  .point {
    width: 4px;
    height: 20px;
    background-color: #ff4136;
    position: absolute;
    top: 0;
  }

  button {
    display: block;
    margin: 20px auto;
    padding: 10px 20px;
    background-color: #007BFF;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .time-points {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 10px;
  }

  .time-point {
    background-color: #f0f0f0;
    border-radius: 5px;
    padding: 5px 10px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
</style>
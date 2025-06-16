<script lang="ts">
  import { onMount } from 'svelte';

  const divider = "|";

  interface TimeZone {
    name: string;
    timeZone: string;
  }

  const timeZones: TimeZone[] = [
    { name: 'New York', timeZone: 'America/New_York' },
    { name: 'London', timeZone: 'Europe/London' },
    { name: 'Tokyo', timeZone: 'Asia/Tokyo' },
    { name: 'Sydney', timeZone: 'Australia/Sydney' }
  ];

  interface DecimalTime {
    time: string;
    dayDifference: number;
    decimalSeconds: number;
  }

  function toDecimalTime(date: Date): DecimalTime {
    const totalSecondsInDay = 24 * 60 * 60;
    const totalDecimalSecondsInDay = 10 * 100 * 100;
    const currentSeconds = date.getHours() * 3600 + date.getMinutes() * 60 + date.getSeconds() + date.getMilliseconds() / 1000;
    const decimalSeconds = (currentSeconds / totalSecondsInDay) * totalDecimalSecondsInDay;

    const decimalHours = Math.floor(decimalSeconds / (100 * 100));
    const decimalMinutes = Math.floor((decimalSeconds % (100 * 100)) / 100);
    const decimalSecondsRemaining = Math.floor(decimalSeconds % 100);

    return {
      time: `${decimalHours}${divider}${decimalMinutes.toString().padStart(2, '0')}${divider}${decimalSecondsRemaining.toString().padStart(2, '0')}`,
      dayDifference: 0, // This will be calculated in getFormattedDecimalTime
      decimalSeconds
    };
  }

  function getFormattedDecimalTime(timeZone: string): DecimalTime {
    const localDate = new Date();
    const targetDate = new Date(localDate.toLocaleString('en-US', { timeZone }));
    const decimalTime = toDecimalTime(targetDate);
    decimalTime.dayDifference = targetDate.getDate() - localDate.getDate();
    return decimalTime;
  }

  let times: DecimalTime[] = timeZones.map(tz => getFormattedDecimalTime(tz.timeZone));

  onMount(() => {
    const interval = setInterval(() => {
      times = timeZones.map(tz => getFormattedDecimalTime(tz.timeZone));
    }, 1000);

    return () => clearInterval(interval);
  });
</script>

<div class="time-zones">
  {#each timeZones as zone, i}
    <div class="time-zone">
      <span class="earth-emoji">🌍</span>
      <h2>{zone.name}</h2>
      <p>
        {times[i].time}
        {#if times[i].dayDifference === 1}
          <span class="next-day">+1 ↻</span>
        {:else if times[i].dayDifference === -1}
          <span class="prev-day">-1 ↺</span>
        {/if}
      </p>
    </div>
  {/each}
</div>

<style>
  .time-zones {
    display: flex;
    flex-direction: column;
    gap: 15px;
    max-width: 300px;
    margin: 0 auto 20px;
  }

  .time-zone {
    background-color: #f0f0f0;
    border-radius: 8px;
    padding: 15px;
    display: flex;
    align-items: center;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  .earth-emoji {
    font-size: 1.5rem;
    margin-right: 10px;
  }

  h2 {
    font-size: 1.2rem;
    margin: 0;
    color: #333;
    flex-grow: 1;
  }

  p {
    font-size: 1.5rem;
    font-weight: bold;
    margin: 0;
    color: #007BFF;
    white-space: nowrap;
  }

  .next-day, .prev-day {
    font-size: 0.9rem;
    margin-left: 5px;
  }

  .next-day {
    color: #ff4500;
  }

  .prev-day {
    color: #4b0082;
  }
  
</style>
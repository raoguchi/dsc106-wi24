<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  import Temperature from './Temperature.svelte';
  import Temperature01Marks from './Temperature01Marks.svelte';
  import Temperature02Channels from './Temperature02Channels.svelte';
  import Temperature03Axes from './Temperature03Axes.svelte';
  import Temperature04Interactive from './Temperature04Interactive.svelte';

  let data = [];

  onMount(async () => {
    const res = await fetch(
      'https://dsc106.com/resources/data/temperatures.csv',
      // 'https://static.observableusercontent.com/files/a6f8018172231770fcb74b515ac8f7d1e40f466f22190c2e89f7621087b4b02a21e5e9a7f20ccb9563055aa1ed16d95b448e7e6dc6d5e7bd096a72a4de0f002d',
    );
    const csv = await res.text();
    await d3.csvParse(csv, (d, i, columns) => {
      for (let i = 1; i < 13; ++i) {
        // pivot longer
        data.push({
          date: new Date(Date.UTC(d.Year, i - 1, 1)),
          value: +d[columns[i]],
        });
      }
    });
    data = data;
  });
</script>

<main>
  <h1>Global Temperature Trends</h1>
  <Temperature {data} />
</main>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Nunito:wght@300;400;700&display=swap');

  :root {
    --color-bg: #ffffff;
    --color-outline: #c2c2c2;
    --color-shadow: hsl(0, 0%, 0%, 0.1);
    --color-text: #3f4252;
    --color-bg-1: hsla(0, 0%, 0%, 0.2);
    --color-shadow-1: hsl(0, 0%, 96%);
  }

  *,
  *::before,
  *::after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  main {
    text-align: center;
    font-family: 'Nunito', sans-serif;
    font-weight: 300;
    line-height: 2;
    font-size: 24px;
    color: var(--color-text);
  }

  h1 {
    font-size: 2em;
    font-weight: 300;
    line-height: 2;
  }
</style>

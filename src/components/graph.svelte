<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  import Graph from './graph.svelte';

  let data = [];
  let tempData = [];

  onMount(async () => {
    const res = await fetch('gdp_co2_death.csv');
    const text = await res.text();
    tempData = d3.csvParse(text, d3.autoType);

    // Parse date fields and create a new 'date' variable
    data = tempData.map(d => ({
      ...d,
      date: new Date(d.Year, 0) // Assuming 'Year' is the name of your date field
    }));
  });
</script>

<main>
  <h3>Topic: Story telling about the Ukraine Conflicts progress</h3>
  <div style="margin: 0 auto; max-width: 950px;">
    <p style="text-align:left; font-size:23px; line-height:100%">
      World Deaths from Conflicts
    </p>
  </div>
  <br>
  <Graph {data} />

  <div style="margin: 0 auto; max-width: 1000px;">
    <div style="margin: 0 auto; max-width: auto;">
      <h4 style="text-align:left;">Write Up: What have you done so far? </h4>
      <p style="text-align:left;">
        We have conducted extensive research on the Ukraine conflicts, gathering data, articles, and reports to gain a comprehensive understanding of the topic.
        We have identified key events, milestones, and developments in the Ukraine conflicts, organizing them into a chronological timeline.
        We have started designing the layout and structure of our interactive web page, envisioning how we will present the information to users.
        We have begun prototyping different visualization techniques, including timeflow charts, maps, and textual narratives, to effectively communicate the progression of the conflicts.
      </p>
      <br>
      <h4 style="text-align:left;">What will be the most challenging of your project to design and why? </h4>
      <p style="text-align:left;">
        we anticipate that integrating the scrollytelling feature will pose the greatest challenge. Scrollytelling involves synchronizing the narrative progression with the user's scrolling actions, creating a seamless storytelling experience. This feature requires precise coordination between text, visuals, and user interactions, ensuring that the content unfolds in a coherent and engaging manner. Additionally, implementing map visualizations with Mapbox presents its own set of challenges, particularly regarding the layout and functionality when the foreground elements are absent. We anticipate that troubleshooting and resolving these issues will require careful attention to detail and experimentation with different design approaches. Despite these challenges, we are committed to delivering a compelling and informative Explorable Explanation on the Ukraine conflicts, leveraging innovative storytelling techniques and data visualization methods.
      </p>
    </div>
    <br>

  </div>
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
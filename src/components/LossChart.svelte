<script>
    import { onMount } from 'svelte';
    import { scaleLinear, scaleBand } from 'd3-scale';
    import { axisLeft } from 'd3-axis';
    import { select } from 'd3-selection';
    import { max, sum } from 'd3-array';
  
    let data = [];
    let selectedDateIndex = 0;
  
    // Function to fetch data from the CSV file
    const fetchData = async () => {
      const response = await fetch('loss.csv');
      const csvData = await response.text();
      const parsedData = d3.csvParse(csvData);
      data = parsedData.map(d => ({
        date: d.date,
        total: sum(Object.keys(d).filter(key => key !== 'date').map(key => +d[key]))
      }));
      renderChart(); // Render the chart initially
    };
  
    // Call the fetchData function on component mount
    onMount(fetchData);
  
    // Function to render the chart
    const renderChart = () => {
      // Retrieve the selected date from the data array using selectedDateIndex
      const selectedDate = data[selectedDateIndex].date;
      const margin = { top: 20, right: 30, bottom: 30, left: 40 };
      const width = 600 - margin.left - margin.right;
      const height = 400 - margin.top - margin.bottom;
  
      const svg = select('.chart')
        .attr('width', width + margin.left + margin.right)
        .attr('height', height + margin.top + margin.bottom)
        .append('g')
        .attr('transform', `translate(${margin.left},${margin.top})`);
  
      const categories = Object.keys(data[0]).filter(key => key !== 'date' && key !== 'total');
      const x = scaleBand()
        .domain(categories)
        .range([0, width])
        .padding(0.1);
  
      const y = scaleLinear()
        .domain([0, max(data, d => d.total)])
        .nice()
        .range([height, 0]);
  
      svg.selectAll('.bar')
        .data(categories)
        .enter().append('rect')
        .attr('class', 'bar')
        .attr('x', d => x(d))
        .attr('y', d => y(data[selectedDateIndex][d]))
        .attr('width', x.bandwidth())
        .attr('height', d => height - y(data[selectedDateIndex][d]))
        .attr('fill', 'steelblue');
  
      // Update y-axis
      svg.append('g')
        .attr('class', 'y-axis')
        .call(axisLeft(y));
    };
  
    // Event handler for slider change
    const handleSliderChange = (event) => {
    selectedDateIndex = parseInt(event.target.value);
    renderChart();
  };
</script>
  
  <div class="chart-container">
    <!-- SVG element for rendering the chart -->
    <svg class="chart"></svg>
  </div>
  
  <!-- Slider for selecting the date -->
  <input type="range" min="0" max="{data.length - 1}" bind:value="{selectedDateIndex}" on:input="{handleSliderChange}">
  
  <style>
    /* Add your styles for the chart here */
  </style>
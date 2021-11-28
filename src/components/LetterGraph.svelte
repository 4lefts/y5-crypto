<script>
  export let width = 950;
  export let height = 400;
  export let labels = [];
  export let data = [];
  import { onMount, tick } from "svelte";
  import { Chart, registerables } from "chart.js";
  Chart.register(...registerables);
  let graphCanvas, graphCtx, graph;

  onMount(() => {
    tick().then(() => {
      graphCtx = graphCanvas.getContext("2d");
      const graph = new Chart(graphCtx, {
        type: "bar",
        data: {
          labels,
          datasets: [
            {
              label: "Frequency of letter",
              data,
              backgroundColor: ["rgba(255, 195, 0, 1)"],
              borderColor: ["rgba(255, 195, 0, 1)"],
              borderWidth: 1,
            },
          ],
        },
        options: {
          scales: {
            y: {
              beginAtZero: true,
            },
          },
        },
      });
    });
  });
</script>

<canvas bind:this={graphCanvas} {width} {height} />

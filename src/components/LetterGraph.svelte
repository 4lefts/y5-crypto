<script>
  export let width = 950;
  export let height = 250;
  export let labels = [];
  export let data = [];
  import { Chart, registerables } from "chart.js";
  Chart.register(...registerables);

  function makeGraph(node, params) {
    let { dataLabels, dataSeries } = params;
    let graphCtx = node.getContext("2d");
    let graph = new Chart(graphCtx, {
      type: "bar",
      data: {
        labels: dataLabels,
        datasets: [
          {
            label: "Frequency of letter",
            data: dataSeries,
            backgroundColor: ["rgba(255, 195, 0, 0.7)"],
            borderWidth: 0,
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

    return {
      update: (newParams) => {
        graph.data.datasets[0].data = newParams.dataSeries;
        graph.update();
      },
    };
  }
</script>

<canvas
  use:makeGraph={{ dataLabels: labels, dataSeries: data }}
  {width}
  {height}
/>

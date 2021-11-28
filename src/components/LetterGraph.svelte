<script>
  export let width = 950;
  export let height = 250;
  export let labels = [];
  export let data = [];
  export let backgroundColor = "rgba(50, 205, 50, 0.7)";
  import { Chart, registerables } from "chart.js";
  Chart.register(...registerables);

  function makeGraph(node, params) {
    let { dataLabels, dataSeries } = params;
    let ctx = node.getContext("2d");
    let graph = new Chart(ctx, {
      type: "bar",
      data: {
        labels: dataLabels,
        datasets: [
          {
            label: "Frequency of letter",
            data: dataSeries,
            backgroundColor,
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
      destroy: () => {
        graph.destroy();
      },
    };
  }
</script>

<canvas
  use:makeGraph={{ dataLabels: labels, dataSeries: data, backgroundColor }}
  {width}
  {height}
/>

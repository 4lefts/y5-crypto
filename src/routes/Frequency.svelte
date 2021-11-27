<script>
  import { Chart, registerables } from "chart.js";
  import { onMount } from "svelte";
  import Header from "../components/Header.svelte";
  import { slide, fade } from "svelte/transition";

  Chart.register(...registerables);

  let canvas, myChart;
  let showInfo = false;
  const alphabet = "abcdefghijklmnopqrstuvwxyz";
  let ciphertext = "";
  const analysis = Array(26).fill(0);
  // const analysis = {
  //   a: 0,
  //   b: 0,
  //   c: 0,
  //   d: 0,
  //   e: 0,
  //   f: 0,
  //   g: 0,
  //   h: 0,
  //   i: 0,
  //   j: 0,
  //   k: 0,
  //   l: 0,
  //   m: 0,
  //   n: 0,
  //   o: 0,
  //   p: 0,
  //   q: 0,
  //   r: 0,
  //   s: 0,
  //   t: 0,
  //   u: 0,
  //   v: 0,
  //   w: 0,
  //   x: 0,
  //   y: 0,
  //   z: 0,
  //   other: 0,
  // };
  function analyze() {
    resetAnalysis();
    const letters = ciphertext.toLowerCase().split("");
    letters.forEach((l, i) => {
      const index = alphabet.indexOf(l);
      if (index === -1) return;
      analysis[index] += 1;
    });
    updateChart(analysis);
  }

  function updateChart(newData) {
    myChart.data.datasets[0].data = newData;
    myChart.update();
  }

  onMount(() => {
    const ctx = canvas.getContext("2d");
    myChart = new Chart(ctx, {
      type: "bar",
      data: {
        labels: alphabet.split(""),
        datasets: [
          {
            label: "Frequency of letter",
            data: analysis,
            backgroundColor: [
              "rgba(255, 99, 132, 0.2)",
              "rgba(54, 162, 235, 0.2)",
              "rgba(255, 206, 86, 0.2)",
              "rgba(75, 192, 192, 0.2)",
              "rgba(153, 102, 255, 0.2)",
              "rgba(255, 159, 64, 0.2)",
            ],
            borderColor: [
              "rgba(255, 99, 132, 1)",
              "rgba(54, 162, 235, 1)",
              "rgba(255, 206, 86, 1)",
              "rgba(75, 192, 192, 1)",
              "rgba(153, 102, 255, 1)",
              "rgba(255, 159, 64, 1)",
            ],
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
  function resetAnalysis() {
    for (const letter in analysis) {
      analysis[letter] = 0;
    }
  }
  function reset() {
    resetAnalysis();
    updateChart(analysis);
    ciphertext = "";
  }
</script>

<Header
  title={"Frequency Analysis"}
  color={"hotpink"}
  {showInfo}
  on:handleShow={() => (showInfo = !showInfo)}
/>

<main in:fade>
  {#if showInfo}
    <div transition:slide>
      <ul>
        <li>Order Of Frequency Of Single Letters</li>
        <li>E T A O I N S H R D L U</li>
        <li>Order Of Frequency Of Digraphs</li>
        <li>
          th er on an re he in ed nd ha at en es of or nt ea ti to it st io le
          is ou ar as de rt ve
        </li>
        <li>Order Of Frequency Of Trigraphs</li>
        <li>the and tha ent ion tio for nde has nce edt tis oft sth men</li>
        <li>Order Of Frequency Of Most Common Doubles</li>
        <li>ss ee tt ff ll mm oo</li>
        <li>Order Of Frequency Of Initial Letters</li>
        <li>T O A W B C D S F M R H I Y E G L N P U J K</li>
        <li>Order Of Frequency Of Final Letters</li>
        <li>E S T D N R Y F L O G H A K M P U W</li>
        <li>One-Letter Words</li>
        <li>a, I</li>
        <li>Most Frequent Two-Letter Words</li>
        <li>
          of, to, in, it, is, be, as, at, so, we, he, by, or, on, do, if, me,
          my, up, an, go, no, us, am
        </li>
        <li>Most Frequent Three-Letter Words</li>
        <li>
          the, and, for, are, but, not, you, all, any, can, had, her, was, one,
          our, out, day, get, has, him, his, how, man, new, now, old, see, two,
          way, who, boy, did, its, let, put, say, she, too, use
        </li>
        <li>Most Frequent Four-Letter Words</li>
        <li>
          that, with, have, this, will, your, from, they, know, want, been,
          good, much, some, time
        </li>
      </ul>
    </div>
  {/if}

  <div class="input-group" style="--focus-color: #00ff00;">
    <label for="ciphertext">Ciphertext</label>
    <textarea
      name="ciphertext"
      placeholder="Encrypted text to analyze"
      title="Paste encrypted text to analyze here."
      bind:value={ciphertext}
    />
    <div class="button-group">
      <button on:click={analyze}>Analyze</button>
      <button on:click={reset}>Reset</button>
    </div>
  </div>
  <canvas bind:this={canvas} width="950" height="400" />
</main>

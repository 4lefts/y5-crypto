<script>
  import { Chart, registerables } from "chart.js";
  import { onMount } from "svelte";
  import Header from "../components/Header.svelte";
  import LetterGraph from "../components/LetterGraph.svelte";
  import { alphabet, letterFrequencies } from "../utils/letter_utils.js";
  import { slide, fade } from "svelte/transition";

  $: letterProportions = letterFrequencies.map((l) => l.proportion);
  Chart.register(...registerables);

  let analysisCanvas, analysisGraph, englishFrequenciesCanvas;
  let showInfo = false;
  let ciphertext =
    "GFS WMY OG LGDVS MF SFNKYHOSU ESLLMRS, PC WS BFGW POL DMFRQMRS, PL OG CPFU M UPCCSKSFO HDMPFOSXO GC OIS LMES DMFRQMRS DGFR SFGQRI OG CPDD GFS LISSO GK LG, MFU OISF WS NGQFO OIS GNNQKKSFNSL GC SMNI DSOOSK. WS NMDD OIS EGLO CKSJQSFODY GNNQKKPFR DSOOSK OIS 'CPKLO', OIS FSXO EGLO GNNQKKPFR DSOOSK OIS 'LSNGFU' OIS CGDDGWPFR EGLO GNNQKKPFR DSOOSK OIS 'OIPKU', MFU LG GF, QFOPD WS MNNGQFO CGK MDD OIS UPCCSKSFO DSOOSKL PF OIS HDMPFOSXO LMEHDS. OISF WS DGGB MO OIS NPHISK OSXO WS WMFO OG LGDVS MFU WS MDLG NDMLLPCY POL LYEAGDL. WS CPFU OIS EGLO GNNQKKPFR LYEAGD MFU NIMFRS PO OG OIS CGKE GC OIS 'CPKLO' DSOOSK GC OIS HDMPFOSXO LMEHDS, OIS FSXO EGLO NGEEGF LYEAGD PL NIMFRSU OG OIS CGKE GC OIS 'LSNGFU' DSOOSK, MFU OIS CGDDGWPFR EGLO NGEEGF LYEAGD PL NIMFRSU OG OIS CGKE GC OIS 'OIPKU' DSOOSK, MFU LG GF, QFOPD WS MNNGQFO CGK MDD LYEAGDL GC OIS NKYHOGRKME WS WMFO OG LGDVS.";
  const frequencies = Array(26).fill(0);

  function analyze() {
    resetAnalysis();
    const letters = ciphertext.toLowerCase().split("");
    letters.forEach((l, i) => {
      const index = alphabet.indexOf(l);
      if (index === -1) return;
      frequencies[index] += 1;
    });
    updateAnalysisGraph(frequencies);
  }

  function updateAnalysisGraph(newData) {
    analysisGraph.data.datasets[0].data = newData;
    analysisGraph.update();
  }

  onMount(() => {
    const analysisCtx = analysisCanvas.getContext("2d");
    analysisGraph = new Chart(analysisCtx, {
      type: "bar",
      data: {
        labels: alphabet,
        datasets: [
          {
            label: "Frequency of letter",
            data: frequencies,
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
        plugins: {
          title: {
            display: true,
            text: "Frequency of letters in Ciphertext",
            align: "start",
          },
        },
      },
    });
  });

  function resetAnalysis() {
    for (const letter in frequencies) {
      frequencies[letter] = 0;
    }
  }

  function reset() {
    resetAnalysis();
    updateAnalysisGraph(frequencies);
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
      <LetterGraph labels={alphabet} data={letterProportions} />
      <dl>
        <dt>Order Of Frequency Of Single Letters</dt>
        <dd>E T A O I N S H R D L U</dd>
        <dt>Order Of Frequency Of Digraphs</dt>
        <dd>
          th er on an re he in ed nd ha at en es of or nt ea ti to it st io le
          is ou ar as de rt ve
        </dd>
        <dt>Order Of Frequency Of Trigraphs</dt>
        <dd>the and tha ent ion tio for nde has nce edt tis oft sth men</dd>
        <dt>Order Of Frequency Of Most Common Doubles</dt>
        <dd>ss ee tt ff ll mm oo</dd>
        <dt>Order Of Frequency Of Initial Letters</dt>
        <dd>T O A W B C D S F M R H I Y E G L N P U J K</dd>
        <dt>Order Of Frequency Of Final Letters</dt>
        <dd>E S T D N R Y F L O G H A K M P U W</dd>
        <dt>One-Letter Words</dt>
        <dd>a, I</dd>
        <dt>Most Frequent Two-Letter Words</dt>
        <dd>
          of, to, in, it, is, be, as, at, so, we, he, by, or, on, do, if, me,
          my, up, an, go, no, us, am
        </dd>
        <dt>Most Frequent Three-Letter Words</dt>
        <dd>
          the, and, for, are, but, not, you, all, any, can, had, her, was, one,
          our, out, day, get, has, him, his, how, man, new, now, old, see, two,
          way, who, boy, did, its, let, put, say, she, too, use
        </dd>
        <dt>Most Frequent Four-Letter Words</dt>
        <dd>
          that, with, have, this, will, your, from, they, know, want, been,
          good, much, some, time
        </dd>
      </dl>
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
  <canvas bind:this={analysisCanvas} width="950" height="400" />
</main>

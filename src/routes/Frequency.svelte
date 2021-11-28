<script>
  import { Chart, registerables } from "chart.js";
  import Header from "../components/Header.svelte";
  import LetterGraph from "../components/LetterGraph.svelte";
  import { alphabetArray, letterFrequencies } from "../utils/letter_utils.js";
  import { slide, fade } from "svelte/transition";

  $: letterFrequenciesArray = letterFrequencies.map((l) => l.frequency); //for English plaintext
  Chart.register(...registerables);

  let showInfo = false;
  let ciphertext =
    "GFS WMY OG LGDVS MF SFNKYHOSU ESLLMRS, PC WS BFGW POL DMFRQMRS, PL OG CPFU M UPCCSKSFO HDMPFOSXO GC OIS LMES DMFRQMRS DGFR SFGQRI OG CPDD GFS LISSO GK LG, MFU OISF WS NGQFO OIS GNNQKKSFNSL GC SMNI DSOOSK. WS NMDD OIS EGLO CKSJQSFODY GNNQKKPFR DSOOSK OIS 'CPKLO', OIS FSXO EGLO GNNQKKPFR DSOOSK OIS 'LSNGFU' OIS CGDDGWPFR EGLO GNNQKKPFR DSOOSK OIS 'OIPKU', MFU LG GF, QFOPD WS MNNGQFO CGK MDD OIS UPCCSKSFO DSOOSKL PF OIS HDMPFOSXO LMEHDS. OISF WS DGGB MO OIS NPHISK OSXO WS WMFO OG LGDVS MFU WS MDLG NDMLLPCY POL LYEAGDL. WS CPFU OIS EGLO GNNQKKPFR LYEAGD MFU NIMFRS PO OG OIS CGKE GC OIS 'CPKLO' DSOOSK GC OIS HDMPFOSXO LMEHDS, OIS FSXO EGLO NGEEGF LYEAGD PL NIMFRSU OG OIS CGKE GC OIS 'LSNGFU' DSOOSK, MFU OIS CGDDGWPFR EGLO NGEEGF LYEAGD PL NIMFRSU OG OIS CGKE GC OIS 'OIPKU' DSOOSK, MFU LG GF, QFOPD WS MNNGQFO CGK MDD LYEAGDL GC OIS NKYHOGRKME WS WMFO OG LGDVS.";
  const frequencies = Array(26).fill(0);
  let doubles = Array(26).fill(0);

  function analyze() {
    resetAnalysis();
    const letters = ciphertext.toLowerCase().split("");
    let previousLetter = "";
    letters.forEach((l) => {
      const index = alphabetArray.indexOf(l);
      // count frequency of doubles
      if (l === previousLetter) {
        doubles[index] += 1;
      }
      previousLetter = l;
      // count freqency of letters
      if (index === -1) return;
      frequencies[index] += 1;
    });
  }

  function resetAnalysis() {
    for (const letter in frequencies) {
      frequencies[letter] = 0;
      doubles[letter] = 0;
    }
  }

  function reset() {
    resetAnalysis();
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
      <p>
        This page has a tool to count the <strong>frequency</strong> of each letter
        and each double letter in a piece of ciphertext. Frequency means how often
        something occurs.
      </p>
      <p>
        One of the problems with the substitution cipher is that it&#39;s
        vulnerable to <strong>frequency analysis</strong>. This means that we
        can start to crack it by counting the letters in the ciphertext, and
        comparing how often different letters appear to how often letters appear
        in plaintext. For example, the most common letter in English is
        &quot;e&quot;. So, if we find out that the most common letter in some
        ciphertext is say, &quot;u&quot;, then we can guess that &quot;e&quot;
        has been encoded to &quot;u&quot;.
      </p>
      <p>This graph shows how common each letter is in English:</p>
      <LetterGraph
        labels={alphabetArray}
        data={letterFrequenciesArray}
        backgroundColor={"rgba(0, 191, 255, 0.7)"}
      />
      <p>
        We can also look for other patterns in English. For example, there are
        only 2 words that have one letter - &quot;a&quot; and &quot;I&quot;.
        And, there aren&#39;t many that have two letters; the most common ones
        (in order of frequency) are:
      </p>
      <ul>
        <li>
          of, to, in, it, is, be, as, at, so, we, he, by, or, on, do, if, me,
          my, up, an, go, no, us, am
        </li>
      </ul>
      <p>
        Another pattern to look for is double letters, as there aren&#39;t many
        of those in English, and they follow patterns (think about where
        &quot;oo&quot; often appears in words). The most common doubles are:
      </p>
      <ul>
        <li>ss ee tt ff ll mm oo</li>
      </ul>
      <p>
        Once you know a few common letters, such as &quot;e&quot; and
        &quot;t&quot;, you can start to work out words like &quot;to&quot; and
        &quot;the&quot; really easily, then start to unpick the whole
        ciphertext.
      </p>
      <p>
        This method is not perfect - you need quite a long piece of ciphertext
        for it work, otherwise there isn&#39;t much point counting the frequecy
        of letters.
      </p>
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
  <h2>Frequency of Each Letter in Ciphertext</h2>
  <LetterGraph labels={alphabetArray} data={frequencies} />
  <h2>Frequency of Double Letters in Ciphertext</h2>
  <LetterGraph
    labels={alphabetArray}
    data={doubles}
    backgroundColor={"rgba(255, 195, 0, 0.7)"}
  />
</main>

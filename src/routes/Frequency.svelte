<script>
  import Header from "../components/Header.svelte";
  import { slide, fade } from "svelte/transition";
  let showInfo = false;
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
      <p>Some info will be here!</p>
      <p>Some info will be here!</p>
      <p>Some info will be here!</p>
      <p>Some info will be here!</p>
      <p>Some info will be here!</p>
    </div>
  {/if}

  <div class="input-group" style="--focus-color: #00ff00;">
    <label for="ciphertext">Ciphertext</label>
    <textarea
      name="ciphertext"
      placeholder="Encrypted text to analyze"
      title="Paste encrypted text to analyze here."
    />
    <div class="button-group">
      <button>Analyze</button>
    </div>
  </div>
</main>

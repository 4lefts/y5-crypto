<script>
  import Header from "../components/Header.svelte";
  import Arrows from "../components/Arrows.svelte";
  import { slide, fade } from "svelte/transition";

  let plaintext = "";
  let ciphertext = "";
  let offset = 1;
  const alphabet = "abcdefghijklmnopqrstuvwxyz";
  let showInfo = false;
  $: key = computeKey(alphabet, offset);

  function computeKey(alphabet, offset) {
    const letters = alphabet.split("");
    return letters
      .map((_, i) => {
        return letters[(i + offset) % 26];
      })
      .join("");
  }

  function encrypt() {
    const letters = plaintext.toLowerCase().split("");
    ciphertext = letters
      .map((l) => {
        const index = alphabet.indexOf(l);
        if (index === -1) return l;
        return key[index];
      })
      .join("");
  }

  function decrypt() {
    const letters = ciphertext.toLowerCase().split("");
    plaintext = letters
      .map((l) => {
        const index = key.indexOf(l);
        if (index === -1) return l;
        return alphabet[index];
      })
      .join("");
  }
</script>

<Header
  title={"Caesar Cipher"}
  color={"tomato;"}
  {showInfo}
  on:handleShow={() => (showInfo = !showInfo)}
/>

<main in:fade>
  {#if showInfo}
    <div transition:slide class="input-group">
      <div class="about">
        <p>
          The <strong>Caesar Cipher</strong> is a simple cipher for
          <strong>encoding</strong>
          and <strong>decoding</strong> messages.
        </p>
        <p>
          It works by shifting the letters of the alphabet a number of places,
          so that each letter changes into another letter. Use the "offset"
          control below to see how this works.
        </p>
        <p>It's obviously not a very strong cipher. Can you see why?</p>
      </div>
      <label for="offset">Offset:</label>
      <input
        style="--focus-color: #0099ff;"
        type="number"
        step="1"
        min="0"
        max="26"
        name="offset"
        title="how far should we shift the alphabet?"
        bind:value={offset}
      />
      <div class="key">
        <div>
          {alphabet}
        </div>
        <Arrows length={alphabet.length} />
        <div>{key}</div>
      </div>
    </div>
  {/if}
  <div class="input-group" style="--focus-color: #00ff00;">
    <label for="plaintext">Plaintext:</label>
    <input
      type="text"
      name="plaintext"
      title="type your plaintext here"
      placeholder="Unencrypted message"
      bind:value={plaintext}
    />
    <div class="button-group">
      <button class="crypt" on:click={encrypt}>Encrypt</button>
      <button class="clear" on:click={() => (plaintext = "")}> Clear</button>
    </div>
  </div>

  <div class="input-group" style="--focus-color: #ff0000;">
    <label for="ciphertext">Ciphertext:</label>
    <input
      type="text"
      name="ciphertext"
      title="type your ciphertext here"
      placeholder="Encrypted message"
      bind:value={ciphertext}
    />
    <div class="button-group">
      <button class="crypt" on:click={decrypt}>Decrypt</button>
      <button class="clear" on:click={() => (ciphertext = "")}>Clear</button>
    </div>
  </div>
</main>

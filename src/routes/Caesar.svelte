<script>
  import Header from "../components/Header.svelte";
  import { slide, fade } from "svelte/transition";

  let plaintext = "";
  let ciphertext = "";
  let offset = 1;
  const alphabet = "abcdefghijklmnopqrstuvwxyz";
  const arrows = Array(alphabet.length).fill("&darr;");
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
  color={"#ff3030;"}
  {showInfo}
  on:handleShow={() => (showInfo = !showInfo)}
/>

<main in:fade>
  {#if showInfo}
    <div transition:slide class="info-group">
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
        min="0"
        max="26"
        name="offset"
        title="how far should we shift the alphabet?"
        bind:value={offset}
      />
      <div class="alphabet">
        <div>
          {alphabet}
        </div>
        <div>
          {#each arrows as arrow}{@html arrow}{/each}
        </div>
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

<style>
  .input-group,
  .info-group {
    display: flex;
    flex-direction: column;
    margin: 1em 0 0 0;
  }

  .info-group {
    margin-bottom: 2em;
  }

  label {
    font-size: 1.6em;
    font-weight: 700;
    text-transform: uppercase;
    margin: 0.7em 0 0.1em 0;
  }

  input {
    padding: 10px;
    height: 50px;
    background-color: whitesmoke;
    border: 2px solid #000;
    text-transform: lowercase;
    box-shadow: 2px 2px black;
    transition: all 0.1s ease;
  }

  input:hover {
    background-color: white;
    box-shadow: 4px 4px black;
  }

  input:focus {
    border-color: var(--focus-color);
    box-shadow: 4px 4px var(--focus-color);
    background-color: white;
    outline: none;
  }

  input[type="number"] {
    width: 140px;
  }

  .button-group {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
  }

  .button-group button {
    width: 140px;
    height: 50px;
    font-weight: 700;
    background-color: whitesmoke;
    border: 2px solid #000;
    box-shadow: 2px 2px black;
    outline: none;
    transition: all 0.3s ease;
  }

  .button-group button:hover {
    cursor: pointer;
    background-color: white;
    box-shadow: 4px 4px black;
  }

  .button-group button:focus,
  .button-group button:active {
    background-color: white;
    border-color: var(--focus-color);
    box-shadow: 4px 4px var(--focus-color);
  }

  .alphabet {
    width: 100%;
    padding: 10px;
    background-color: whitesmoke;
    font-family: monospace;
    font-size: 2em;
    letter-spacing: 0.4em;
    border: 2px solid #000;
    box-shadow: 2px 2px black;
  }
</style>

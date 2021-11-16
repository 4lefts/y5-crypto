<script>
  import { onMount } from "svelte";
  import Header from "../components/Header.svelte";
  let plaintext = "";
  let ciphertext = "";
  const alphabet = "abcdefghijklmnopqrstuvwxyz";
  let key = "";
  let showInfo = false;
  let isRemovingSpaces = false;

  onMount(() => {
    computeKey();
  });

  function computeKey() {
    let letters = alphabet.split("");
    for (let index = alphabet.length; index > 0; index--) {
      let randomIndex = Math.floor(Math.random() * index);
      let randomValue = letters[randomIndex];
      let indexValue = letters[index];
      letters[index] = randomValue;
      letters[randomIndex] = indexValue;
    }
    key = letters.join("");
  }

  function encrypt() {
    const letters = plaintext.toLowerCase().split("");
    ciphertext = letters
      .filter((l) => {
        if (isRemovingSpaces) {
          console.log(isRemovingSpaces);
          return l != " ";
        }
        return true;
      })
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
  title={"Substitution Cipher"}
  color={"dodgerblue"}
  {showInfo}
  on:handleShow={() => (showInfo = !showInfo)}
/>

<main>
  {#if showInfo}
    <button on:click={computeKey}> Randomize Key </button>

    <div style="font-family: monospace">
      <div>
        {alphabet}
      </div>
      <div>
        {key}
      </div>
    </div>

    <label for="spaces">Remove Spaces?</label>
    <input
      id="spaces"
      title="remove spaces from ciphertext?"
      type="checkbox"
      bind:value={isRemovingSpaces}
      on:click={() => (isRemovingSpaces = !isRemovingSpaces)}
    />
  {/if}
  <label for="plaintext">Plain text:</label>
  <input
    type="text"
    name="plaintext"
    title="type your plain text here"
    bind:value={plaintext}
  />
  <button on:click={encrypt}>Encrypt</button>
  <button on:click={() => (plaintext = "")}>Clear</button>

  <label for="ciphertext">Cipher text:</label>
  <input
    type="text"
    name="ciphertext"
    title="type your cipher text here"
    bind:value={ciphertext}
  />
  <button on:click={decrypt}>Decrypt</button>
  <button on:click={() => (ciphertext = "")}>Clear</button>
</main>

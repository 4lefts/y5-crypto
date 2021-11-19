<script>
  import { onMount } from "svelte";
  import Header from "../components/Header.svelte";
  let plaintext = "";
  let ciphertext = "";
  const alphabet = "abcdefghijklmnopqrstuvwxyz";
  let key = "";
  let showInfo = true;
  let isRemovingSpaces = false;

  onMount(() => {
    computeKey();
  });

  function computeKey() {
    let letters = alphabet.split("");
    // knuth shuffle of letters
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
    <div class="input-group">
      <div class="about">
        <p>
          The <strong>Substitution Cipher</strong> is another simple cipher for
          <strong>encoding</strong> and <strong>decoding</strong> messages.
        </p>
        <p>
          Like the Caesar Cipher, it works by shifting the letters of the
          alphabet so that each letter changes into another letter, but the
          difference this time is that order of the letters is now random. So,
          instead of each letter just moving along <em>in order</em>, it now
          changes into another <em>random</em> letter.
        </p>
        <p>Use the "randomise key" button to generate a new random key.</p>
        <p>
          It's a bit stronger that the Caesar Cipher, but not by much. Can you
          see why?
        </p>
      </div>
      <button on:click={computeKey}> Randomize Key </button>
      <div style="font-family: monospace">
        <div class="key">
          <div>
            {alphabet}
          </div>
          <div>
            {key}
          </div>
        </div>
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
  <div class="input-group" style="--focus-color: #00ff00;">
    <label for="plaintext">Plain text:</label>
    <input
      type="text"
      name="plaintext"
      title="type your plain text here"
      placeholder="Unencrypted message"
      bind:value={plaintext}
    />
    <div class="button-group">
      <button on:click={encrypt}>Encrypt</button>
      <button on:click={() => (plaintext = "")}>Clear</button>
    </div>
  </div>

  <div class="input-group" style="--focus-color: #ff0000;">
    <label for="ciphertext">Cipher text:</label>
    <input
      type="text"
      name="ciphertext"
      title="type your cipher text here"
      placeholder="Encrypted message"
      bind:value={ciphertext}
    />
    <div class="button-group">
      <button on:click={decrypt}>Decrypt</button>
      <button on:click={() => (ciphertext = "")}>Clear</button>
    </div>
  </div>
</main>

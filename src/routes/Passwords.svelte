<script>
  import { fade, slide } from "svelte/transition";
  import Header from "../components/Header.svelte";
  import { alphabetString } from "../utils/letter_utils";
  let showInfo = false;
  let password = "";
  let guess = "";
  let duration = 0;

  function crackPassword() {
    duration = 0;
    guess = "";
    const start = new Date().getTime();
    // this is obviously wrong - we need to pad out the whole string of characters
    for (const passwordLetter of password) {
      for (const alphabetLetter of alphabetString) {
        console.log(`${passwordLetter} = ${alphabetLetter}`);
        if (passwordLetter === alphabetLetter) {
          guess += alphabetLetter;
          console.log(guess);
          break;
        }
      }
    }
    const end = new Date().getTime();
    duration = end - start;
  }
</script>

<Header
  title={"Password Cracking"}
  {showInfo}
  color={"darkorchid"}
  on:handleShow={() => (showInfo = !showInfo)}
/>

<main>
  {#if showInfo}
    <div transition:slide>
      <p>Some info will be here!</p>
    </div>
  {/if}
  <div class="input-group" style="--focus-color: darkorchid;">
    <label for="password">Password</label>
    <input
      name="password"
      type="password"
      title="enter a password to test"
      placeholder="enter a password"
      bind:value={password}
    />
    <div class="button-group">
      <button on:click={crackPassword}>Crack</button>
    </div>
  </div>
  <pre>{password}</pre>
  <!-- {#if guess.length} -->
  <h2>Guess is {guess}</h2>
  <!-- {/if} -->
  {#if duration > 0}
    <h2>That took {duration / 1000} seconds</h2>
  {/if}
</main>

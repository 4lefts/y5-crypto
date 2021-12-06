<script>
  import { tick } from "svelte";

  import { fade, slide } from "svelte/transition";
  import Header from "../components/Header.svelte";
  import { alphabetArray } from "../utils/letter_utils";
  let showInfo = false;
  let password = "";
  $: if (password.length > 7) {
    password = password.slice(0, 7);
  }

  let guess = "";
  let guessing = false;
  let found = false;
  let duration = 0;
  $: seconds = (duration / 1000) % 60;
  $: minutes = Math.floor(duration / 60000);

  function findPassword() {
    // helper function to recursively generate
    // all possible passwords up to length len
    // from a set of possibleCharacters
    // takes
    // possibleCharacters: e.g. alphabet
    // str: current password being generated
    // len: the length of the password being generated
    // size: size of the possibleCharacters array
    function iteratePasswords(possibleCharacters, str, len, size) {
      if (len == 0) {
        if (str == password) {
          guess = str;
          found = true;
        }
        return; // base case for recursion
      }
      for (let i = 0; i < size; i++) {
        // recusively call this function,abcde
        // to append the possibleCharacters to the guess
        iteratePasswords(
          possibleCharacters,
          str + possibleCharacters[i],
          len - 1,
          size
        );
        if (found) {
          break;
        }
      }
    }
    const start = new Date().getTime();
    found = false;
    let i = 1;
    while (!found) {
      iteratePasswords(alphabetArray, "", i, alphabetArray.length);
      i++;
    }
    const end = new Date().getTime();
    duration = end - start;
    guessing = false;
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
      <h2 id="cracking-passwords">Cracking Passwords</h2>
      <p>
        Why is it important to have a strong password? What might happen if
        someone knew your passwords? What could they do?
      </p>
      <p>
        One way for people to break into your accounts is by guessing your
        password. One way to guess passwords is to try <em>every</em> possible
        password. This is called a <strong>brute force</strong> approach, because
        someone is using the computer is just trying to smash their way into your
        account, without thinking too much about how they could make the job easier.
      </p>
      <p>
        It works like this:<br />Let&#39;s say that passwords only contain
        letters a-z, with no upper case letters, numbers or other symbols. A
        brute force approach starts by trying the password &quot;a&quot;, and
        seeing if that works, then &quot;b&quot;, &quot;c&quot; and
        &quot;d&quot; and so on, all the way up to &quot;z&quot;. Then, it tries
        all the two-letter passwords: &quot;aa&quot;, &quot;ab&quot;,
        &quot;ac&quot;, up to &quot;az&quot;, then &quot;ba&quot;,
        &quot;bb&quot; until eventually, it gets to &quot;zz&quot;, then it
        moves on to all the three-letter passwords, then four, then five, until
        eventually it finds a match.
      </p>
      <p>
        The tool on this page will do this for passwords containing lower-case
        letters (a-z), up to 8 characters in length. What do you notice as the
        passwords get longer? As passwords get longer, the amount of time it
        takes increases <strong>exponentially</strong>, so usually, hackers
        don&#39;t take just a brute force approach. But, you can still stop this
        kind of problem by having longer passwords.
      </p>
      <p>
        Please remember that this is just a simple tool. If your computer is not
        very powerful, it might start running really slowly as it tries to crack
        a longer password, and cracking a longer password might take a while!
      </p>
    </div>
  {/if}
  <div class="input-group" style="--focus-color: darkorchid;">
    <label for="password"
      >Password <span>Maximum 8 characters, lowercase letters only</span></label
    >
    <input
      name="password"
      type="password"
      title="enter a password to test"
      placeholder="enter a password"
      bind:value={password}
    />
    <div class="length-display">Length: {password.length}</div>
    <div class="length-display">Password: {password}</div>
    <div class="button-group">
      <button
        on:click={async () => {
          guessing = true;
          duration = 0;
          guess = "";
          setTimeout(() => {
            findPassword();
          }, 1000);
        }}>Crack</button
      >
    </div>
  </div>
  {#if guessing}<h2>Thinking...</h2>{/if}
  {#if guess}<h2>The password is {guess}</h2>{/if}
  {#if duration > 0}
    <div transition:fade class="duration-display">
      That took: {minutes} minutes {seconds} seconds
    </div>
  {/if}
</main>

<style>
  label > span {
    margin-left: 10px;
    font-size: 0.7em;
    text-transform: none;
    color: dimgray;
  }
  .length-display,
  .duration-display {
    color: dimgray;
    padding: 0.2em 0 0.8em 0;
    font-weight: 700;
  }
</style>

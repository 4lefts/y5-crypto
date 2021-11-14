<script>
  import { fade } from "svelte/transition";
  const vocab = [
    {
      term: "Cipher",
      definition: "An agreed algorithm for encrypting of decrypting a message",
    },
    {
      term: "Ciphertext",
      definition:
        "Information (a message) that has been encrypted so that it can't be read without the key",
    },
    {
      term: "Codes",
      definition:
        "Ways of changing how information is presented - they don't have to be secret",
    },
    {
      term: "Cryptanalysis",
      definition:
        "Working out how to decrypt and encrypt messages without having the key",
    },
    {
      term: "Cryptography",
      definition: "The science of keeping communication and information secret",
    },
    {
      term: "Decrypt",
      definition:
        "Converting an encrypted message (ciphertext) into readable text (plaintext)",
    },
    {
      term: "Encode",
      definition:
        "Changing a message into a code - it doesn't have to be secret",
    },
    {
      term: "Encrypt",
      definition:
        "Converting a readable message (plaintext) into an encrypted message (ciphertext) so that it cannot be read without the key",
    },
    {
      term: "Message",
      definition:
        "Information that is to be transmitted (sent) from one person (or system) to another",
    },
    {
      term: "Morse Code",
      definition:
        "A simple code for converting letters and numbers into patterns of long and short electrical pulses",
    },
    {
      term: "Plaintext",
      definition:
        "Information (a message) that is not encrypted, so that anyone can read it",
    },
    {
      term: "Semaphore",
      definition:
        "A simple code for converting letters and numbers into different positions of two flags, one held in each hand",
    },
    {
      term: "Transmit",
      definition: "To communicate over a long distance",
    },
  ];
</script>

<svelte:head>
  <title>Vocabulary</title>
</svelte:head>

<div class="header-container" style={`--header-color: lightseagreen;`}>
  <header>
    <a in:fade href="/">
      <img src="icons/arrow_back_black_24dp.svg" alt="go back" />
    </a>
    <h1>Useful Vocabulary</h1>
  </header>
</div>

<main in:fade>
  <ul>
    {#each vocab as { term, definition }}
      <li>
        <h3>{term}</h3>
        <p>{definition}</p>
      </li>
    {/each}
  </ul>
</main>

<style>
  h1 {
    flex-grow: 2;
    margin: 0 10px 0 -40px;
    text-transform: uppercase;
    text-align: center;
  }
  ul {
    padding: 0;
  }
  li {
    list-style-type: none;
    border: 2px solid lightseagreen;
    box-shadow: 2px 2px lightseagreen;
    padding: 0.1em 20px;
    margin: 10px 0 20px 0;
  }
</style>

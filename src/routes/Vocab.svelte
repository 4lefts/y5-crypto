<script>
  import { fade, slide } from "svelte/transition";
  const vocab = [
    {
      term: "Cipher",
      isOpen: false,
      definition: "An agreed algorithm for encrypting of decrypting a message",
    },
    {
      term: "Ciphertext",
      isOpen: false,
      definition:
        "Information (a message) that has been encrypted so that it can't be read without the key",
    },
    {
      term: "Codes",
      isOpen: false,
      definition:
        "Ways of changing how information is presented - they don't have to be secret",
    },
    {
      term: "Cryptanalysis",
      isOpen: false,
      definition:
        "Working out how to decrypt and encrypt messages without having the key",
    },
    {
      term: "Cryptography",
      isOpen: false,
      definition: "The science of keeping communication and information secret",
    },
    {
      term: "Decrypt",
      isOpen: false,
      definition:
        "Converting an encrypted message (ciphertext) into readable text (plaintext)",
    },
    {
      term: "Encode",
      isOpen: false,
      definition:
        "Changing a message into a code - it doesn't have to be secret",
    },
    {
      term: "Frequency",
      isOpen: false,
      definition:
        "How often something happens or appears, such as how often a letter appears in a piece of text",
    },
    {
      term: "Encrypt",
      isOpen: false,
      definition:
        "Converting a readable message (plaintext) into an encrypted message (ciphertext) so that it cannot be read without the key",
    },
    {
      term: "Message",
      isOpen: false,
      definition:
        "Information that is to be transmitted (sent) from one person (or system) to another",
    },
    {
      term: "Morse Code",
      isOpen: false,
      definition:
        "A simple code for converting letters and numbers into patterns of long and short electrical pulses",
    },
    {
      term: "Plaintext",
      isOpen: false,
      definition:
        "Information (a message) that is not encrypted, so that anyone can read it",
    },
    {
      term: "Semaphore",
      isOpen: false,
      definition:
        "A simple code for converting letters and numbers into different positions of two flags, one held in each hand",
    },
    {
      term: "Transmit",
      isOpen: false,
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
    {#each vocab as { term, isOpen, definition }}
      <li on:click={() => (isOpen = !isOpen)}>
        <h3>{term}</h3>
        {#if isOpen}<p transition:slide={{ duration: 200 }}>
            {definition}
          </p>{/if}
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
    cursor: pointer;
    transition: all 0.2s ease;
  }
  li:hover {
    box-shadow: 4px 4px lightseagreen;
  }
</style>

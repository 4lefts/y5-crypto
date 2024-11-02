<script>
  import { fade } from "svelte/transition";
</script>

<svelte:head>
  <title>Y5 Cryptography</title>
</svelte:head>

<div class="header-container" style={`--header-color: darkgrey;`}>
  <header>
    <h1>Year 5 - We Are Cryptographers</h1>
  </header>
</div>

<main in:fade>
  <p>
    Here are some little apps to help explore the cryptography algorithms and
    code-breaking that we are learning about in school. Hopefully they're
    useful!
  </p>
  <p>
    They are hugely indebted to <a href="https://simonsingh.net/"
      >Simon Singh's</a
    >
    website
    <a href="https://www.simonsingh.net/The_Black_Chamber/index.html"
      >The Black Chamber</a
    >.
  </p>

  <nav>
    <!-- add class="not-yet" to these links to make inactive -->
    <a href="/#/caesar-cipher" style="--link-color: tomato">1. Caesar Cipher</a>
    <a href="/#/substitution-cipher" style="--link-color: dodgerblue" class="not-yet"
      >2. Substitution Cipher</a
    >
    <a href="/#/frequency-analysis" style="--link-color: hotpink" class="not-yet"
      >3. Frequency Analysis</a
    >
    <a href="/#/password-cracking" style="--link-color: darkorchid" class="not-yet"
      >4. Password Cracking</a
    >
    <a href="/#/vocab" style="--link-color: teal">Useful Vocabulary</a>
  </nav>
</main>

<style>
  h1 {
    flex-grow: 2;
    margin: 0 10px;
    text-transform: uppercase;
    text-align: center;
  }

  nav a {
    position: relative;
    display: block;
    padding: 0.6em 10px;
    text-decoration: none;
    font-size: 1.3em;
    font-weight: 700;
    color: var(--link-color);
    background-color: white;
    border: 2px solid var(--link-color);
    box-shadow: 2px 2px var(--link-color);
    transition: all 0.3s ease;
    margin: 10px 0;
  }

  nav a:hover {
    cursor: pointer;
    color: white;
    background-color: var(--link-color);
    border-color: white;
    box-shadow: 4px 4px var(--link-color);
  }

  nav a.not-yet {
    background-color: #f8f8f8;
    color: #dcdcdc;
    border-color: #dcdcdc;
    box-shadow: none;
  }

  nav a.not-yet:hover {
    cursor: default;
  }

  nav a.not-yet::after {
    content: "Coming soon!";
    position: absolute;
    bottom: 5px;
    right: 20px;
    color: var(--link-color);
    filter: opacity(50%);
  }
</style>

<script>
  import Router from "svelte-spa-router";
  import Home from "./routes/Home.svelte";
  import Vocab from "./routes/Vocab.svelte";
  import Caesar from "./routes/Caesar.svelte";
  import Substitution from "./routes/Substitution.svelte";
  import Frequency from "./routes/Frequency.svelte";
  import Passwords from "./routes/Passwords.svelte";

  const routes = {
    "/": Home,
    "/vocab": Vocab,
    "/caesar-cipher": Caesar,
    // "/substitution-cipher": Substitution,
    // "/frequency-analysis": Frequency,
    // "/password-cracking": Passwords,
    "*": Home,
  };
</script>

<Router {routes} />
<footer>
  <p>By Stephen Ball</p>
</footer>

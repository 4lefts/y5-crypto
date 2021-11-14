<script>
  import Header from "../components/Header.svelte";
  let showInfo = false;
</script>

<Header
  title={"Frequency Analysis"}
  {showInfo}
  on:handleShow={() => (showInfo = !showInfo)}
/>

<main>
  {#if showInfo}
    <p>Some info will be here!</p>
  {/if}
</main>

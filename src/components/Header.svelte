<script>
  import { createEventDispatcher } from "svelte";
  import { fade } from "svelte/transition";
  export let title;
  export let color;
  export let showInfo;
  const dispatch = createEventDispatcher();
</script>

<svelte:head>
  <title>{title}</title>
</svelte:head>

<div class="header-container" style={`--header-color: ${color}`}>
  <header>
    <a in:fade href="/">
      <img src="icons/arrow_back_black_24dp.svg" alt="go back" />
    </a>
    <h1>{title}</h1>
    <button in:fade on:click={() => dispatch("handleShow")}>
      {#if showInfo}
        <img
          src="icons/close_black_24dp.svg"
          alt="close information and controls"
        />
      {:else}
        <img src="icons/info_black_24dp.svg" alt="show info and controls" />
      {/if}
    </button>
  </header>
</div>

<style>
  h1 {
    flex-grow: 2;
    margin: 0 10px;
    text-transform: uppercase;
    text-align: center;
  }
</style>

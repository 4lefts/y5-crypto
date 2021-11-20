<script>
  export let length;
  const arrows = Array(length).fill("&darr;");
</script>

<div>
  {#each arrows as arrow}{@html arrow}{/each}
</div>

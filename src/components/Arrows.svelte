<script>
  const arrows = Array(26).fill("&darr;");
</script>

<div>
  {#each arrows as arrow}{@html arrow}{/each}
</div>

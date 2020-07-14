<script>
  import Title from "./Title.svelte";
  import Gasto from "./Gasto.svelte";
  import { fly } from "svelte/transition";
  import { flip } from "svelte/animate";
  export let expenses = [];
</script>
<style>
  .mensaje-sin-notas{
    display: flex;
    flex-direction: column;
    justify-content: center;
    text-align: center;
    color: rgb(73, 73, 73);
  }
  h1{
    margin-bottom: 2rem;
    color: rgb(255, 183, 0);
  }
</style>
<div>

  <ul>
    {#each expenses as expense, index (expense.id)}
      <div in:fly={{ x: 200, delay: index * 200 }} out:fly={{ x: -200 }} animate:flip>
        <Gasto index={index + 1} {expense} />
      </div>
    {:else}
    <div class="mensaje-sin-notas">
      <h1>K-Keeps</h1>
      <h2>Las notas que añadas apararecerán aquí</h2>
    </div>
    {/each}
  </ul>

</div>

<script>
  import { blur, slide, scale, fade, fly } from "svelte/transition";
  import { quintOut } from "svelte/easing";
  import { getContext } from "svelte";
  import { autoResize } from "./autoResize.js";

  export let index;
  export let expense;
  let EstadoAmount = false;

  function toggleAmount() {
    EstadoAmount = !EstadoAmount;
  }

  const removeExpense = getContext("remove");
  const setEditExpense = getContext("edit");
</script>

<style>
  .nota-vacia {
    margin: 10px 0;
  }
</style>

<article use:autoResize class="single-expense" style="background-color:{expense.color}">

  <div class="expense-info">
    {#if !expense.name == '' && !expense.descripcion == ''}
      <h2>
        {expense.name}
        {#if expense.amount != null}
          <button class="amount-btn" on:click={toggleAmount}>
            <i class="fas fa-caret-down" />
          </button>
        {/if}
      </h2>
      <p>{expense.descripcion}</p>
      {#if EstadoAmount}
        <h4 transition:slide>amount: ${expense.amount}</h4>
      {/if}
    {:else}
      <h1 class="nota-vacia">Nota vacia</h1>
    {/if}

  </div>
  <div class="form__utilidades">
    <div class="form__utilidades__item">
      <button
        class="form__utilidades__item--icon "
        on:click={setEditExpense(expense.id)}>
        <i class="fas fa-pen" />
      </button>
      <span class="form__utilidades__item--mensaje">Editar</span>
    </div>
    <div class="form__utilidades__item">
      <button
        class="form__utilidades__item--icon"
        on:click={removeExpense(expense.id)}>
        <i class="fas fa-trash" />
      </button>
      <span class="form__utilidades__item--mensaje">Eliminar</span>
    </div>
  </div>
</article>

<script>
  import { blur, slide, scale, fade, fly } from "svelte/transition";
  import { quintOut } from "svelte/easing";
  import { getContext } from "svelte";

  export let index;
  export let expense;
  let EstadoAmount = false;

  function toggleAmount() {
    EstadoAmount = !EstadoAmount;
  }

  const removeExpense = getContext("remove");
  const setEditExpense = getContext("edit");
</script>

<article class="single-expense">

  <div class="expense-info">
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
  </div>
  <div class="form__utilidades">
    <div class="form__utilidades__item">
      <button
        class="form__utilidades__item--icon "
        on:click={setEditExpense(expense.id)}>
        <i class="fas fa-pen" />
      </button>
      <span class="form__utilidades__item--mensaje">
        Editar
      </span>
    </div>
    <div class="form__utilidades__item">
      <button
        class="form__utilidades__item--icon"
        on:click={removeExpense(expense.id)}>
        <i class="fas fa-trash" />
      </button>
      <span class="form__utilidades__item--mensaje">
        Eliminar 
      </span>
    </div>
  </div>
</article>

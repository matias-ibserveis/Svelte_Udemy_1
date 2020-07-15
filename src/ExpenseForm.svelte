<script>
  /* import { onMount, onDestroy, beforeUpdate, afterUpdate } from "svelte";
  
  onMount(() => {
    console.log("Form ha sido montado");
  });
  beforeUpdate(() => {
    console.log("before Update");
  });
  afterUpdate(() => {
    console.log("after update");
  });
  onDestroy(() => {
    console.log("form is hidden");
  });
   */
  import { autoResize } from "./autoResize.js";
  import Title from "./Title.svelte";
  import Todos from "./Todos.svelte";

  export let name = "";
  export let amount = null;
  export let descripcion = "";
  export let color = "#ffffff";
  
  export let addExpense;
  export let paleta_colores;


  $: isEmpty = !name || !descripcion;

  function handleSubmit() {
    addExpense(name, amount, descripcion, color);
    name = "";
    amount = null;
    descripcion = "";
    color = "#ffffff";
  }
</script>

<style>
  .form {
    position: relative;
    margin-bottom: 5rem;
  }
  .expense-form {
    background: var(--mainWhite);
    padding: 7px 10px;
    text-transform: capitalize;
    border-radius: 8px;
    box-shadow: 0 1px 2px 0 rgba(60, 64, 67, 0.302),
      0 2px 6px 2px rgba(31, 87, 129, 0.149);
  }
  .input-precio {
    width: 100px !important;
    border: 1px solid rgb(175, 175, 175) !important;
    border-radius: 4px;
  }

  .form-control input,
  textarea {
    height: auto;
    resize: none;
    color: rgb(51, 51, 51);
    width: 100%;
    border: none;
    margin-bottom: 2.9rem;
    padding: 0.5rem;
    font-size: 0.9em;
    font-weight: 500;
    font-family: "Open Sans", sans-serif;
  }
  .expense-form .form-control:first-child > input {
    margin-bottom: 5px;
    font-weight: 600;
    font-size: 1em;
    color: rgb(31, 31, 31);
  }
  .form-empty {
    text-align: center;
    color: red;
    margin: 0;
  }
  .btn-add {
    position: absolute;
    right: 15px;
    bottom: -20px;
    cursor: pointer;
    background-color: rgb(94, 160, 94);
    border: none;
    border-radius: 100%;
    padding: 17px 20px;
    color: white;
    transition: all 0.4s ease;
  }
  .btn-add:hover {
    background-color: rgb(0, 184, 0);
  }
  .close-btn {
    position: absolute;
    top: 0;
    right: 0;
    font-size: 1.2rem;
    text-transform: capitalize;
    display: flex;
    align-items: center;
    width: 4.2rem;
    justify-content: space-between;
    background: transparent;
    border: none;
    color: red;
    cursor: pointer;
  }

  .active {
    border: 1px solid black !important;
  }
</style>

<section class="form">
  <form
    style="background-color:{color}"
    class="expense-form"
    on:submit|preventDefault={handleSubmit}>
    <div class="form-control">
      <input
        style="background-color:{color}"
        type="text"
        id="name"
        bind:value={name}
        autocomplete="off"
        placeholder="Título" />
    </div>
    <div class="form-control">
      <textarea
        style="background-color:{color}"
        type="text"
        bind:value={descripcion}
        use:autoResize
        id="amount"
        autocomplete="off"
        placeholder="Añade una nota..." />
    </div>
    <div class="form__utilidades">
      <div class="form__utilidades__item">
        <div class="form__utilidades__item--icon">
          <i class="fas fa-dollar-sign" />
        </div>
        <div class="form__utilidades__item--utilidad">
          <input
            class="input-precio"
            type="number"
            id="amount"
            bind:value={amount}
            autocomplete="off"
            placeholder="Precio" />
        </div>
      </div>
      <div class="form__utilidades__item">
        <div class="form__utilidades__item--icon">
          <i class="fas fa-palette" />
        </div>
        <div class="form__utilidades__item--utilidad">
          <ul class="color-note">
            {#each paleta_colores as paleta_color, index}
              <div class="color-note__group">
                <input
                  type="radio"
                  value={paleta_color}
                  bind:group={color}
                  name={paleta_color} />
                <label
                  for={paleta_color}
                  class="color-note__item"
                  style="color:transparent; background-color: {paleta_color};
                  border:1px solid rgb(216, 216, 216);">
                  {paleta_color}
                </label>
              </div>
            {/each}
          </ul>
        </div>
      </div>
    </div>
    <!-- {#if isEmpty}
      <p class="form-empty">Por favor rellena bien el formulario</p>
    {/if} -->
    <button
      type="submit"
      class="btn-add"
      class:disabled={isEmpty}
      disabled={isEmpty}>
      <i class="fas fa-plus" />
    </button>

    <!-- <button type="button" class="close-btn" on:click={hideForm}>
      <i class="fas fa-times" />
      <span>Cerrar</span>
    </button> -->

  </form>

</section>

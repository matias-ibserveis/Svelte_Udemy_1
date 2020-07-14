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
  export let isEditing;
  export let editExpense;
  export let hideForm;
  export let clearData;

  if (name) {
    console.log("verdadero");
  }

  $: isEmpty = !name || !descripcion;

  function handleSubmit() {
    editExpense({ name, amount, descripcion });
   
    name = "";
    amount = null;
    descripcion = "";
    hideForm();
    clearData()
  }
</script>

<style>
  .form {
    position: relative;
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
  .btn-cerrar {
    position: absolute;
    bottom: 5px;
    right: 15px;
    padding: 10px 20px;
    font-size: 0.9em;
    font-weight: 500;
    border: none;
    color: rgb(51, 51, 51);
    cursor: pointer;
    border-radius: 4px;
  }
  .btn-cerrar:hover{
    background-color: rgb(235, 235, 235);
  }
  .form__utilidades {
    position: absolute;
    bottom: 5px;
    display: flex;
  }
  .form__utilidades__item {
    position: relative;
    margin: 0 1px;
    cursor: pointer;
    transition: all 0.1s;
  }
  .form__utilidades__item--icon {
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 100%;
    width: 32px;
    height: 32px;

    color: rgb(90, 90, 90);
  }
  .form__utilidades__item:hover {
  }
  .form__utilidades__item:hover > .form__utilidades__item--icon {
    background-color: rgb(238, 238, 238);
  }
  .form__utilidades__item:hover > .form__utilidades__item--utilidad {
    display: block;
  }
  .form__utilidades__item--utilidad {
    display: none;
    position: absolute;
    top: 40px;
    left: 5px;
    background-color: rgb(247, 247, 247);
    padding: 30px 20px;
    z-index: 11111;
  }
  .form__utilidades__item--utilidad input {
    color: rgb(51, 51, 51);
    width: 100%;
    border: none;
    margin-bottom: 5px;
    padding: 0.5rem;
    font-size: 0.9em;
    font-weight: 500;
    font-family: "Open Sans", sans-serif;
  }
</style>

<section class="form">
  <form class="expense-form" on:submit|preventDefault={handleSubmit}>
    <div class="form-control">
      <input
        type="text"
        id="name"
        bind:value={name}
        autocomplete="off"
        placeholder="Título" />
    </div>
    <div class="form-control">
      <textarea
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
      </div>
    </div>
    <!-- {#if isEmpty}
      <p class="form-empty">Por favor rellena bien el formulario</p>
    {/if} -->
    
      <button
        type="submit"
        class="btn-cerrar"
        disabled={isEmpty}>
        Cerrar
      </button>
    
    <!-- <button type="button" class="close-btn" on:click={hideForm}>
      <i class="fas fa-times" />
      <span>Cerrar</span>
    </button> -->

  </form>

</section>

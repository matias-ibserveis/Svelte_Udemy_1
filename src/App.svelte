<script>
  import Github from "./Github.svelte";
  import Githubawait from "./Githubawait.svelte";

  import { onMount, afterUpdate } from "svelte";
  import { setContext } from "svelte";
  /* const state= {
    name:"simple name",
    remove: removeExpense
  } */
  import Gasto from "./Gasto.svelte";

  // Componentes
  import Modal from "./Modal.svelte";
  import Navbar from "./Navbar.svelte";
  import ListaGastos from "./ListaGastos.svelte";
  import Todos from "./Todos.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  import FormEdit from "./FormEdit.svelte";
  // Data
  import Gastos from "./expenses.js";

  // Variables
  let expenses = [];
  let paleta_colores = [
    "#ffffff",
    "#f28b82",
    "#fbbc04",
    "#fff475",
    "#ccff90",
    "#a7ffeb",
    "#cbf0f8",
    "#aecbfa",
    "#d7aefb",
    "#fdcfe8",
    "#e6c9a8",
    "#e8eaed",
  ];

  //set editing variables
  let setName = "";
  let setAmount = null;
  let setId = null;
  let setDescripcion = "";
  let setColor = "";

  //toggle

  let isFormOpen = false;

  // Reactive
  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((acc, curr) => {
    return (acc += curr.amount);
  }, 0);

  //functions

  function showForm() {
    isFormOpen = true;
  }
  function hideForm() {
    isFormOpen = false;
  }
 

  function removeExpense(id) {
    expenses = expenses.filter((item) => item.id !== id);
  }
  function clearExpenses() {
    let alerta = confirm("seguro que deseas eliminar todas las notas?");
    alerta ? (expenses = []) : "";
  }
  function addExpense(name, amount, descripcion, color) {
    let expense = {
      id: Math.random() * Date.now(),
      name,
      descripcion,
      amount,
      color,
    };
    expenses = [expense, ...expenses];
  }
  function setEditExpense(id) {
    /* "filtramos" con el metodo find()
      y obtenemos todas la propiedades de dicho objeto por su ID. */
    let expense = expenses.find((item) => item.id === id);

    // los valores obtenidos, los asignamos a la variables set.
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    setDescripcion = expense.descripcion;
    setColor = expense.color;
    console.log(setColor);
    showForm();
  }
  function editExpense({ name, amount, descripcion, color }) {
    console.log(name, amount, descripcion, color)
    expenses = expenses.map((item) => {
      return item.id === setId
        ? { ...item, name, amount, descripcion, color }
        : { ...item };
    });
    console.log(expenses)
    console.log(color)
  }

  setContext("remove", removeExpense);
  setContext("edit", setEditExpense);

  // Local Storage
  function setLocalStorage() {
    localStorage.setItem("gastos", JSON.stringify(expenses));
  }

  onMount(() => {
    expenses = localStorage.getItem("gastos")
      ? JSON.parse(localStorage.getItem("gastos"))
      : [];
  });

  afterUpdate(() => {
    console.log("after update");
    setLocalStorage();
  });
</script>

<style>
  .overflowHidden {
    overflow: hidden;
  }
  .btn-borrarTodasNotas {
    cursor: pointer;
    width: 100%;
    background-color: #2b2b2b;
    padding: 13px;
    color: rgb(255, 183, 0);
    font-size: 1em;
    font-weight: 500;
    text-transform: uppercase;
    letter-spacing: 1px;
    border: none;
    border-radius: 4px;
  }
  .btn-borrarTodasNotas:hover {
    background-color: #414141;
  }
</style>

<!-- <svelte:body style={isFormOpen ? 'overflowHidden' : ''} /> -->
<Navbar {showForm} />
<main class="content">
  <ExpenseForm {paleta_colores} {addExpense}   />
  <!--   <Githubawait /> -->
  {#if isFormOpen}
    <Modal>
      <FormEdit
        {paleta_colores}
        name={setName}
        amount={setAmount}
        descripcion={setDescripcion}
        color={setColor}
        {editExpense}
        {hideForm} />
    </Modal>
  {/if}
  <!-- <Todos title="Gastos Totales" {total} /> -->
  <ListaGastos {expenses} />
  {#if expenses.length >= 1}
    <button type="button" class="btn-borrarTodasNotas" on:click={clearExpenses}>
      Borrar todas las notas
    </button>
  {/if}

</main>

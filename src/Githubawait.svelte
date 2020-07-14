<script>
  async function getUsers() {
    let userData = await fetch("https://api.github.com/users");
    console.log(userData);
    let githubUsers = await userData.json();
    console.log(githubUsers);
    return githubUsers;
  }
</script>

<section>
  {#await getUsers()}
    <h1>Loading...</h1>
  {:then users}
    {#each users as user}
      <article class="user">
        <img src={user.avatar_url} alt="" width="50" />
        <div class="user-info">
          <h3>User: {user.login}</h3>
          <a href={user.html_url} class="btn-primary">Ir al Github</a>
        </div>
      </article>
    {/each}
    {:catch error}
    <h2>Ocurrio un error : {error.message}</h2>
  {/await}
</section>

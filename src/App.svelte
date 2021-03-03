<script>
  import { onMount } from "svelte";
  import auth from "./authService";
  import { isAuthenticated, user, user_tasks, tasks } from "./store";
  import AppMainData from "./FunctionalComponents/AppMainData.svelte";

  let auth0Client;

  onMount(async () => {
    auth0Client = await auth.createClient();

    isAuthenticated.set(await auth0Client.isAuthenticated());
    user.set(await auth0Client.getUser());
  });

  function login() {
    auth.loginWithPopup(auth0Client);
  }

  function logout() {
    auth.logout(auth0Client);
  }
</script>

<main>
  <!-- App Bar -->
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <a class="navbar-brand" href="/#">Task Manager</a>

    <div class="collapse navbar-collapse" id="navbarText">
      <div class="navbar-nav mr-auto user-details">
        {#if $isAuthenticated}
          <span class="text-white"
            >&nbsp;&nbsp;{$user.name} ({$user.email})</span
          >
        {:else}<span>&nbsp;</span>{/if}
      </div>
      <span class="navbar-text">
        <ul class="navbar-nav float-right">
          {#if $isAuthenticated}
            <li class="nav-item">
              <a class="nav-link" href="/#" on:click={logout}>Log Out</a>
            </li>
          {:else}
            <li class="nav-item">
              <a class="nav-link" href="/#" on:click={login}>Log In</a>
            </li>
          {/if}
        </ul>
      </span>
    </div>
  </nav>

  <!-- Application -->
  {#if !$isAuthenticated}
    <div class="container mt-5">not logged</div>
  {:else}
    <AppMainData />
  {/if}
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

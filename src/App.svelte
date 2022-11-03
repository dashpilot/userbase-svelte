<script>
import { onMount } from 'svelte';

  let mode = 'signin';
  let username, password
  // will be set by userbase
  let userObj = null
  const userbase = window.userbase
  // Userbase initializer
  let authProm = userbase.init({appId: appId})
    .then(({user}) => userObj = user)
  

  const signIn = () => authProm = userbase.signIn({username, password}).then(user => userObj = user)
  const signOut = () => authProm = userbase.signOut().then(() => userObj = null)
  const signUp = () => authProm = userbase.signUp({username, password}).then(user => userObj = user )
  
  
  onMount(async () => {
  document.addEventListener("keyup", function(event) {
      if (event.keyCode === 13) {
          if(mode == 'signin'){
          signIn();
        }else{
          signUp();
      
      }
    }
  });
  });
  
  
</script>

  <nav class="navbar navbar-dark bg-dark mb-5">
  <div class="row w-100">
    <div class="col-6 text-start brand">
    <span class="navbar-brand h1">{appName}</span>
    </div>
    <div class="col-6 text-end">
      
 {#if userObj}
 <button class="btn btn-outline-light w-25" on:click={signOut}>Sign Out</button>
 {/if}
    </div>
    </div>
</nav>
  


{#await authProm}

<div class="card">
  Loading...
</div>
{:then}


  {#if !userObj}
  
    <div class="card">
    <input placeholder="email" type="email" bind:value={username}><br>
    <input placeholder="password" type="password" bind:value={password}><br>
   

    {#if showSignUp}
    
    {#if mode == 'signin'}
    <button on:click={signIn}>Sign In</button>
    {:else}
    <button on:click={signUp}>Sign Up</button>	
    {/if}
    
    {/if}
    
    </div>
    
    {#if showSignUp}
    
    {#if mode == 'signin'}
    
    <div class="link">
    No account yet? <a on:click="{() => mode = 'signup'}">Sign Up</a>.
    </div>
    {:else}
    
    <div class="link">
    Already have an account? <a on:click="{() => mode = 'signin'}">Sign In</a>.
    </div>
    {/if}
    
    {/if}
    

  {:else}
  

  
  <div class="card">
 
    
  </div>
  {/if}

{:catch error}
 <div class="card">
  <h5 class="error">{error.message}</h5>
 </div>
 
 <div class="link"><a on:click="{() => location.reload()}">&laquo; back</a></div>
{/await}




<style>
  .error {
    color: red;
    margin-top: 5px;
  }
  h3 {
    color: blue;
  }
  
  .navbar{
    height: 55px;
  }
  
  .brand{

    padding-left: 35px;
  }
  
  .btn-outline-light{
  width: 100px !important;
    padding-top: 2px;
    padding-bottom: 2px;
    margin: 0;
    font-size: 13px;
  }
 
</style>
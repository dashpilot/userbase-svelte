<script>
  import Userbase from "./components/Userbase.svelte"
  let userObj = null;
  let dbName = "default"
  let posts = [];
  
  let title;
  
  async function addItem(){
    
    userbase.insertItem({
      databaseName: dbName,
      item: { title: title }
    }).then(() => {
      // item inserted
      console.log('inserted')
    }).catch((e) => console.error(e))
  
  }
  
  async function deleteItem(id){
    if(confirm('Are you sure you wish to delete this item?')){
      userbase.deleteItem({
        databaseName: dbName,
        itemId: id
      }).then(() => {
        // item deleted
        console.log('deleted')
      }).catch((e) => console.error(e))
    }
  }
</script>

<Userbase bind:userObj bind:dbName bind:posts />
  
  
  {#if userObj}
  <div class="card">
  
  <input type="text" bind:value={title}>
  
  <button on:click={addItem}>Add</button>
  
  {#if posts.length}
  <ul class="list-group mt-3">
  {#each posts as post}
  <li class="list-group-item text-start">
    
    <span class="text-start">{post.item.title}</span>
    <span class="text-end float-end"><i class="fas fa-trash" on:click={deleteItem(post.itemId)}></i></span>
    
  </li>
  {/each}
  </ul>
  {/if}
  
  
  </div>

  {/if}
<script>
    import supabase from '$lib/db';

    let word;

    // SELECT word
    async function getSecretWord() {
   	 const { data, error } = await supabase.from('secret').select();
   	 if (error) alert(error.message);

   	 return data;
    }

    // UPSERT word
    async function saveSecretWord() {
   	 const { error } = await supabase
   		 .from('secret')
   		 .upsert(
   		     { user_id: supabase.auth.user().id, word: word },
   		     { onConflict: 'user_id' }
   		 );
   	 if (error) alert(error.message);

   	 location.reload(); // Refresh the page.
    }

    // Sign Out
    async function signOut() {
   	 const { error } = await supabase.auth.signOut();
   	 if (error) alert(error.message);
    }
</script>

<div class="text-center">
    <h1 class="bg-warning p-5">Welcome!</h1>

    {#await getSecretWord()}
   	 <p>Fetching data...</p>
    {:then data}
   	 {#each data as secret}
   		 <h3 class="text-success my-3">Your secret word: {secret.word}</h3>
   	 {/each}
    {:catch error}
   	 <p>Something went wrong while fetching the data:</p>
   	 <pre>{error}</pre>
    {/await}

    <div class="text-center mb-5">
   	 <form on:submit|preventDefault={saveSecretWord}>
   		 <label for="word">Your secret word:</label>
   		 <input id="word" bind:value={word} type="text" />

   		 <button>Save</button>
   	 </form>
    </div>

    <button on:click={signOut}>Sign out</button>
</div>
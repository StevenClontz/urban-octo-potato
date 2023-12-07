<script lang="ts">
    import { onMount } from "svelte";
    import { supabase } from "../supabaseClient";
  
    export let id: string
    let loading = false
    let name: string | null = null
  
    onMount(() => {
      getOutcome()
    })
  
    const getOutcome = async () => {
      try {
        loading = true
  
        const { data, error, status } = await supabase
          .from('outcomes')
          .select('name')
          .eq('id', id)
          .single()
  
        if (error && status !== 406) throw error
  
        if (data) {
          name = data.name
        }
      } catch (error) {
        if (error instanceof Error) {
          alert(error.message)
        }
      } finally {
        loading = false
      }
    }
  
  </script>
  
  {#if loading}
  Loading!
  {:else}
  <form on:submit|preventDefault={()=>'foo'} class="form-widget">
     <h3>Outcome Name: {name}</h3>
    <!--<div>
      <label for="username">Name</label>
      <input id="username" type="text" bind:value={username} />
    </div>
    <div>
      <label for="website">Website</label>
      <input id="website" type="text" bind:value={website} />
    </div>
    <div>
      <button type="submit" class="button primary block" disabled={loading}>
        {loading ? 'Saving ...' : 'Update profile'}
      </button>
    </div> -->
  </form>
  {/if}
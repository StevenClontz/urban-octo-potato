<script lang="ts">
  import { onMount } from 'svelte';
  import { supabase } from "./supabaseClient";
  import Outcome from './lib/Outcome.svelte';
  let loading = false
  let outcomes: {id:string}[] | null = null

  onMount(() => {
    getOutcomes()
  })

  const getOutcomes = async () => {
    try {
      loading = true

      const { data, error, status } = await supabase
        .from('outcomes')
        .select('id')

      if (error && status !== 406) throw error

      if (data) {
        outcomes = data
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
<main>
  <div>
    {#if outcomes}
      {#each outcomes as outcome}
        <Outcome id={outcome.id}/>
      {/each}
    {/if}
  </div>
</main>
{/if}


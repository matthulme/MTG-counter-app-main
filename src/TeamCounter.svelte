<script>
   import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    export let playing

    export let team

    $: ({id, name, score} = team)
    $: color = name.toLowerCase()

    function updateScore(action) {
        dispatch('updateScore', {
            id,
            updateAction: action
        })
    }
</script>

<div class="text-center p-3" style="border: 5px dashed {color}">
    <h2 style="color: {color};">Team {name}: {score}</h2>
    <button class="btn btn-success" on:click={() => updateScore('add')} disabled={!playing}>+</button>
    <button class="btn btn-danger" on:click={() => updateScore('minus')} disabled={!playing}>-</button>
</div>

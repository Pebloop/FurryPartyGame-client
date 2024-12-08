<script lang="ts">

    import RunRunRunGame from "$lib/games/runrunrun/RunRunRunGame.svelte";

    export let game: String;

</script>

{#if game === 'RunRunRun'}
    <RunRunRunGame />
{:else}
    <h1>Game not found</h1>
{/if}
<script lang="ts">

    import RunRunRunGame from "$lib/games/runrunrun/RunRunRunGame.svelte";
    import DictionaryGame from "$lib/games/dictionary/DictionaryGame.svelte";

    export let game: String;
    export let player: String;
    export let code: String;
    export let socket: WebSocket;

</script>

{#if game === 'RunRunRun'}
    <RunRunRunGame {player} {code} {socket} />
{:else if game === 'Dictionary'}
    <DictionaryGame {player} {code} {socket} />
{:else}
    <h1>Game not found</h1>
{/if}
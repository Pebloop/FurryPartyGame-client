<script lang="ts">

    export let player: String;
    export let code: String;
    export let socket: WebSocket;

    function jump() {
        console.log('Jumping');
        socket.send(JSON.stringify({
            type: "runrunrun_jump",
            player: player,
            code: code
        }));
    }

    function StartCrouching() {
        console.log('Crouching');
        socket.send(JSON.stringify({
            type: "runrunrun_start_crouching",
            player: player,
            code: code
        }));
    }

    function StopCrouching() {
        console.log('Stop Crouching');
        socket.send(JSON.stringify({
            type: "runrunrun_stop_crouching",
            player: player,
            code: code
        }));
    }

</script>

<div>
    <h1>Run Run Run</h1>
    <h1>Player: {player}</h1>
    <h1>Code: {code}</h1>
    <button id="jump" on:click={jump}>Jump</button>
    <button id="crouch" on:mousedown={StartCrouching} on:mouseup={StopCrouching}>Crouch</button>
</div>
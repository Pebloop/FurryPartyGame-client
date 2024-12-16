<script lang="ts">

    import {onMount} from "svelte";

    export let player: String;
    export let code: String;
    export let socket: WebSocket;

    let isCrouching = false;

    onMount(() => {
        // jump if space is pressed
        window.addEventListener('keydown', (e) => {
            if (e.code === 'Space') {
                jump();
            }
            if (e.code === 'ShiftLeft') {
                crouch();
            }
        });
    });

    function jump() {
        console.log('Jumping');
        socket.send(JSON.stringify({
            time: Date.now(),
            type: "runrunrun_jump",
            player: player,
            code: code
        }));
    }

    function crouch() {
        console.log('Crouching');
        if (!isCrouching) {
            StartCrouching();
            isCrouching = true;
        } else {
            StopCrouching();
            isCrouching = false;
        }
    }

    function StartCrouching() {
        console.log('Crouching');

        socket.send(JSON.stringify({
            time: Date.now(),
            type: "runrunrun_start_crouching",
            player: player,
            code: code
        }));
    }

    function StopCrouching() {
        console.log('Stop Crouching');
        socket.send(JSON.stringify({
            time: Date.now(),
            type: "runrunrun_stop_crouching",
            player: player,
            code: code
        }));
    }

</script>

<div class="flex flex-col items-center w-full h-screen">
    <div class="flex flex-row justify-between w-full p-2 bg-gray-600 text-white h-10">
        <h1>Run Run Run !</h1>
        <h1>{player}</h1>
        <h1>{code}</h1>
    </div>
    <div class="flex flex-row justify-between w-full p-2 items-center full_height buttons">
        <button id="jump" on:click={jump} class="w-1/2 m-3 bg-gray-500 rounded-2xl text-white h-full button">Jump</button>
        <button id="crouch" on:click={crouch} class="w-1/2 bg-gray-500 rounded-2xl text-white h-full button">Crouch</button>
    </div>
</div>

<style>
    .full_height {
        height: calc(100% - 40px);
    }

    @media (max-width: 600px) {
        .buttons {
            flex-direction: column;
        }

        .button {
            height: 50%;
            width: 100%;
        }
    }
</style>
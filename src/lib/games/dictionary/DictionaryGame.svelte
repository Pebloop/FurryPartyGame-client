<script lang="ts">

    import {onMount} from "svelte";

    export let player: String;
    export let code: String;
    export let socket: WebSocket;

    let currentWord = "";

    onMount(() => {
        // Listen for the Enter key to send the word
        window.addEventListener('keydown', (e) => {
            if (e.code === 'Enter') {
                sendWord();
            }
        });

        socket.onmessage = (e) => {
            console.log("Received message", e);
        }
    });

    function sendWord() {
        console.log('sending word');
        socket.send(JSON.stringify({
            type: "dictionary_send_word",
            word: currentWord,
            player: player,
            code: code
        }));
    }


</script>

<div class="flex flex-col items-center w-full h-screen">
    <div class="flex flex-row justify-between w-full p-2 bg-gray-600 text-white h-10">
        <h1>Dictionary</h1>
        <h1>{player}</h1>
        <h1>{code}</h1>
    </div>
    <div class="flex flex-col justify-between w-full p-2 items-center full_height buttons">
        <input type="text" id="word" placeholder="Enter a word" class="mb-2 rounded-2xl h-8 w-full pl-2" bind:value={currentWord}/>
        <button id="crouch" on:click={sendWord} class="w-1/2 bg-gray-500 rounded-2xl text-white h-full button">send</button>
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
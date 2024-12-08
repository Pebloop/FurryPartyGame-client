<script lang="ts">
    import {onMount} from "svelte";
    import GameScreen from "$lib/games/GameScreen.svelte";

    let name = '';
    let room = '';

    let gameName = "";

    let socket = null;

    onMount(() => {
        let popup = document.getElementById('popup');
        popup.style.display = 'none';

        let game = document.getElementById('game');
        game.style.display = 'none';

        socket = new WebSocket('wss://ws-fpg.pebloop.dev');

        socket.onopen = function (event) {
            console.log('Connected to server');
        };

        socket.onmessage = function (event) {
            console.log('Message from server ', event.data);

            const data = JSON.parse(event.data);

            switch (data.type) {
                case 'room_joined':
                    document.getElementById('join-form').remove();
                    game.style.display = 'flex';
                    gameName = data.game;
                    break;
                default:
                    popup.style.display = 'flex';
                    break;
            }
        };

        socket.onclose = function (event) {
            console.log('Disconnected from server');
        };

    });

    function join() {
        if (name && room) {
            socket.send(JSON.stringify({
                type: 'join',
                name,
                room
            }));
        }
    }
</script>

<div class="flex flex-col items-center justify-center h-screen" id="join-form">
    <div class="flex flex-col w-80 bg-gray-500 rounded-2xl items-center">
        <img src="/logo.png" alt="logo" class="w-32"/>
        <div class="flex flex-col p-4 w-full items-center">
            <input type="text" id="name" placeholder="Enter your name" value={name} on:change={e => name = e.target.value} class="mb-2 rounded-2xl h-8 w-full pl-2"/>
            <input type="text" id="room" placeholder="Enter room code" value={room} on:change={e => room = e.target.value} class="mb-2 rounded-2xl h-8 w-full pl-2"/>
            <button id="join" on:click={join} class="bg-blue-500 text-white rounded-2xl h-8 w-1/3 mt-3">Join</button>
        </div>
    </div>
</div>

<div id="popup" class="hidden">
    <div class="flex flex-col items-center justify-center h-screen absolute top-0 left-0 w-full h-full bg-black bg-opacity-50">
        <div class="flex flex-col w-80 bg-gray-500 rounded-2xl items-center">
            <div class="flex flex-col p-4 w-full items-center">
                <h1 class="text-white text-xl">Room could not be joined.</h1>
                <button id="close" on:click={() => document.getElementById('popup').style.display = 'none'} class="bg-blue-500 text-white rounded-2xl h-8 w-1/3 mt-3">Close</button>
            </div>
        </div>
    </div>
</div>

<div id="game">
    <GameScreen game={gameName}/>
</div>
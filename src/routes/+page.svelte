<script lang="ts">
    import {onMount} from "svelte";

    let name = '';
    let room = '';

    let socket = null;

    onMount(() => {
        socket = new WebSocket('wss://ws-fpg.pebloop.dev');

        socket.onopen = function (event) {
            console.log('Connected to server');
        };

        socket.onmessage = function (event) {
            console.log('Message from server ', event.data);
        };

        socket.onclose = function (event) {
            console.log('Disconnected from server');
        };

        socket.addEventListener('error', function (event) {
            console.log('Error: ', event);
        });
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

<div class="flex flex-col" id="join-form">
    <h1>Furry Party Game</h1>
    <input type="text" id="name" placeholder="Enter your name" value={name} on:change={e => name = e.target.value}/>
    <input type="text" id="room" placeholder="Enter room code" value={room} on:change={e => room = e.target.value}/>
    <button id="join" on:click={join}>Join</button>
</div>
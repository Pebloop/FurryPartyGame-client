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

            const data = JSON.parse(event.data);

            switch (data.type) {
                case 'join':
                    if (data.success) {
                        // remove join form
                        document.getElementById('join-form').remove();

                        // show room
                    } else {
                        console.log('Failed to join room');
                    }
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
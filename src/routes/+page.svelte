<script lang="ts">
    let name = '';
    let room = '';

    let socket = null;

    if (document) {
        socket = new WebSocket('ws://146.59.153.243:8001');
    }

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

<div class="flex flex-col">
<h1>Furry Party Game</h1>
<input type="text" id="name" placeholder="Enter your name" value={name} on:change={e => name = e.target.value}/>
<input type="text" id="room" placeholder="Enter room code" value={room} on:change={e => room = e.target.value}/>
<button id="join" on:click={join}>Join</button>
</div>
<script>
    import { onMount } from 'svelte';

    let darkTheme = false;

    function toggleTheme() {
        darkTheme = !darkTheme;
    }

    function leaveRoom() {
        alert('Leaving room...');
        window.location.href = '/src/routes/(tools)/disposable-chatroom/+page.svelte';
    }

    function attachDocument() {
        const input = document.createElement('input');
        input.type = 'file';
        input.accept = '.pdf,.doc,.docx,.txt,.jpg,.png';

        input.onchange = function(e) {
            const file = e.target.files[0];
            if (!file) return;
            alert('Selected file: ' + file.name);
        };
        input.click();
    }

    let showEmojiPicker = false;
    let message = '';

    function insertEmoji(emoji) {
        message += emoji;
    }
</script>

<style>
    @import url('https://cdn.jsdelivr.net/npm/@xz/fonts@1/serve/plus-jakarta-display.min.css');
    @import url('https://fonts.googleapis.com/css2?family=Noto+Color+Emoji&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Kalam:wght@300;400;700&display=swap');
    @import url('https://fonts.cdnfonts.com/css/cascadia-code');

    body {
        margin: 0;
        font-family: 'Plus Jakarta Display', sans-serif, 'Noto Color Emoji';
        transition: background-color 0.3s, color 0.3s;
        display: flex;
        height: 100vh;
        background-color: #fff;
        color: #000;
    }
    .dark-theme {
        background-color: #000;
        color: #fff;
    }
    .chat-container {
        display: flex;
        flex-direction: column;
        flex-grow: 1;
        height: 100%;
    }
    .header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 20px;
        background-color: #fff;
        border-bottom: 1px solid #ccc;
    }
    .dark-theme .header {
        background-color: #000;
        border-bottom-color: #000;
    }
    .header .room-name {
        font-size: 1.5em;
        font-weight: bold;
        color: #aa0ca5;
    }
    .header .controls {
        display: flex;
        align-items: center;
    }
    .header .theme-toggle,
    .header .leave-room {
        font-size: 1.5em;
        cursor: pointer;
        margin-left: 15px;
    }
    .header .leave-room {
        color: red;
    }
    .messages-area {
        flex-grow: 1;
        padding: 20px;
        overflow-y: auto;
        background-color: #fff;
    }
    .dark-theme .messages-area {
        background-color: #222;
    }
    .message-input {
        display: flex;
        padding: 10px;
        border-top: 1px solid #ccc;
        background-color: #fff;
        align-items: center;
        position: relative;
    }
    .dark-theme .message-input {
        background-color: #000;
        border-top-color: #000;
    }
    .message-input input[type="text"] {
        flex-grow: 1;
        padding: 10px;
        border: 1px solid #333;
        border-radius: 5px;
        font-size: 1em;
    }
    .dark-theme .message-input input[type="text"] {
        background-color: #555;
        border-color: #666;
        color: #fff;
    }
    .message-input button {
        padding: 10px 20px;
        margin-left: 10px;
        border: none;
        background-color: #007bff;
        color: #fff;
        border-radius: 5px;
        cursor: pointer;
        font-size: 1em;
        transition: 0.3s;
    }
    .dark-theme .message-input button {
        background-color: #28a745;
    }
    .message-input button:hover {
        background-color: #0056b3;
    }
    .dark-theme .message-input button:hover {
        background-color: #218838;
    }
    .participants-container {
        display: flex;
        flex-direction: column;
        width: 250px;
    }
    .participants-header {
        padding: 20px;
        background-color: #fff;
        border-bottom: 1px solid #ccc;
        text-align: center;
        font-size: 1.5em;
        font-weight: bold;
        color: #aa0ca5;
    }
    .dark-theme .participants-header {
        background-color: #000;
        border-bottom-color: #000;
    }
    .participants-list {
        flex-grow: 1;
        padding: 10px;
        background-color: #f5f5f5;
        border-left: 1px solid #ccc;
        display: flex;
        flex-direction: column;
    }
    .dark-theme .participants-list {
        background-color: #000;
        border-left-color: #000;
    }
    .participant {
        display: flex;
        align-items: center;
        margin-bottom: 10px;
    }
    .participant img {
        width: 40px;
        height: 40px;
        border-radius: 50%;
        margin-right: 10px;
    }
    .participant .status {
        width: 10px;
        height: 10px;
        border-radius: 50%;
        margin-right: 10px;
    }
    .status.online {
        background-color: green;
    }
    .status.offline {
        background-color: red;
    }
    .dark-theme .participant .status.online {
        background-color: lightgreen;
    }
    .dark-theme .participant .status.offline {
        background-color: darkred;
    }
    .dark-theme .participant img {
        filter: brightness(0.8);
    }
    .emoji-button {
        padding: 10px 15px;
        border: none;
        background-color: #f0f0f0;
        color: #666;
        border-radius: 5px;
        cursor: pointer;
        font-size: 1.2em;
        transition: 0.3s;
    }
    .emoji-button:hover {
        background-color: #e0e0e0;
    }
    .dark-theme .emoji-button {
        background-color: #333;
        color: #ccc;
    }
    .dark-theme .emoji-button:hover {
        background-color: #444;
    }
    .attach-icon {
        padding: 10px;
        border-right: 1px solid #ccc;
        cursor: pointer;
    }
    .attach-icon i {
        font-size: 1.5em;
        color: #666;
    }
    .dark-theme .attach-icon {
        border-color: #666;
    }
    .dark-theme .attach-icon i {
        color: #ccc;
    }
    .emoji-picker {
        position: absolute;
        bottom: 60px;
        left: 10px;
        width: 200px;
        background-color: #fff;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
        border-radius: 5px;
        padding: 10px;
        display: none;
    }
    .dark-theme .emoji-picker {
        background-color: #333;
    }
    .emoji-picker button {
        font-size: 1.5em;
        cursor: pointer;
        background: none;
        border: none;
        margin: 5px;
    }
</style>

<div class="chat-container {darkTheme ? 'dark-theme' : ''}">
    <header class="header">
        <div class="room-name">Room Name</div>
        <div class="controls">
            <span class="theme-toggle" on:click={toggleTheme}>
                {#if darkTheme}
                    🌙
                {:else}
                    ☀️
                {/if}
            </span>
            <span class="leave-room" on:click={leaveRoom}>Leave Room</span>
        </div>
    </header>
    <div class="chat-content">
        <div class="participants-container">
            <div class="participants-header">Participants</div>
            <div class="participants-list">
                <!-- Placeholder participants list -->
                <div class="participant">
                    <div class="status online"></div>
                    <img src="https://via.placeholder.com/40" alt="User Image" />
                    <span>User 1</span>
                </div>
                <div class="participant">
                    <div class="status offline"></div>
                    <img src="https://via.placeholder.com/40" alt="User Image" />
                    <span>User 2</span>
                </div>
            </div>
        </div>
        <div class="messages-area">
            <!-- Placeholder for messages -->
            <div class="message">
                <p>Welcome to the chat!</p>
            </div>
        </div>
        <div class="message-input">
            <button class="emoji-button" on:click={() => showEmojiPicker = !showEmojiPicker}>😀</button>
            <input type="text" bind:value={message} placeholder="Type a message..." />
            <button>Send</button>
        </div>
    </div>
    <div class="emoji-picker" style="display: {showEmojiPicker ? 'block' : 'none'};">
        <button on:click={() => insertEmoji('😀')}>😀</button>
        <button on:click={() => insertEmoji('😁')}>😁</button>
        <button on:click={() => insertEmoji('😂')}>😂</button>
        <button on:click={() => insertEmoji('🤣')}>🤣</button>
        <button on:click={() => insertEmoji('😃')}>😃</button>
    </div>
</div>

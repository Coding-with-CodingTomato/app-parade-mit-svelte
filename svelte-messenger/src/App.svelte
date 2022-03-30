<script>
  import GUN from "gun";

  let gun = GUN(["https://zerrays-gun-server.herokuapp.com/gun"]);

  let messages = gun.get("ct-messages");
  console.log(messages);

  let allMessages = [];
	messages.map().on((m, id) => {
		const newMessage = {
			id: id,
			username: m.username,
			message: m.message,
			timeSent: m.timeSent,
		};

		allMessages.push(newMessage);
		allMessages = allMessages;
	});

	allMessages = allMessages.sort((a, b) => {
		if(a.timeSent < b.timeSent) return -1;
		if(a.timeSent > b.timeSent) return 1;
		return 0;
	});

	console.log(allMessages);

  let username = "";
  let tempUsername = "";
  let messageInput = "";

  const sendMessage = () => {
    if (messageInput.trim() === "") return;

    messages.set({ username, message: messageInput, timeSent: Date.now(), });
		messageInput = "";
    console.log("Message sent");
  };
</script>

<h1>Svelte GUN.js Messenger</h1>
<main>
  {#if username !== ""}
    <article class="chatWrapper">
      {#each allMessages || [] as { message, username }}
        <div class="message">
          <p>
            <b>{username}:</b>
            <span>{message}</span>
          </p>
        </div>
      {/each}
    </article>
    <p>Dein Username: {username}</p>
    <input type="text" bind:value={messageInput} maxlength="200" placeholder="Nachricht" />
    <input type="button" on:click={sendMessage} value="Senden" />
  {:else}
    <p>Gib deinen Username ein:</p>
    <input type="text" bind:value={tempUsername} maxlength="20" placeholder="Usernamen" />
    <input
      type="button"
      on:click={() => (username = tempUsername)}
      value="Speichern"
    />
  {/if}
</main>

<style>
  h1 {
    margin-bottom: 0;
  }
  .chatWrapper {
    height: 40vh;
    width: 100%;
    display: flex;
    flex-direction: column;
    overflow-y: auto;
    margin-top: 0;
  }
</style>

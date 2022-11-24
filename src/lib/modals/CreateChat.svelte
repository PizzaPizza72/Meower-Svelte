<script>	
	import Modal from "../Modal.svelte";

	import {modalShown, mainPage} from "../stores.js";

    import * as clm from "../clmanager.js";

	import {tick} from "svelte";

    let createStatus = "";

	function doCreate(nickname) {
		try {
			createStatus = "Création de chat...";
			clm.meowerRequest({
				cmd: "direct",
				val: {
					cmd: "create_chat",
					val: nickname,
				},
			}).then(() => {
				modalShown.set(false);
				mainPage.set("blank");
				tick().then(() => mainPage.set("chatlist"));
			}).catch(code => {
				createStatus = `Erreur ${code} inattendue !`;
			});
		} catch(e) {
			console.error(e);
			createStatus = "Erreur de connexion : " + e;
		}
	}
</script>

<Modal on:close={() => {$modalShown = false}}>
    <h2 slot="header">Créer une conversation</h2>
    <div slot="default">
		<form
			on:submit|preventDefault={e => {
				if (!e.target[0].value) {
					createStatus = "Vous devez spécifier un pseudo pour créer le chat !";
					return false;
				}
				doCreate(
					e.target[0].value,
				);
				return false;
			}}
		>
			{#if createStatus}
			<span class="login-status">{createStatus}</span><br />
			{/if}
			<!-- svelte-ignore a11y-autofocus -->
			<input type="text" class="modal-input white" placeholder="Surnom" maxlength="20" autofocus><br /><br />
			<div class="modal-buttons">
				<button type="button" on:click={() => {$modalShown = false}}>Annuler</button>
				<button type="submit">Créer</button>
			</div>
		</form>
	</div>
</Modal>
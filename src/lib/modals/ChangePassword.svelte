<script>	
	import Modal from "../Modal.svelte";

	import {screen, setupPage, modalShown} from "../stores.js";

    import * as clm from "../clmanager.js";

	import {tick} from "svelte";

    let changeStatus = "";
</script>

<Modal on:close={() => {$modalShown = false}}>
    <h2 slot="header">Changer le mot de passe</h2>
    <div slot="default">
		<form
			on:submit|preventDefault={async e => {
				if (!e.target[0].value) {
					changeStatus = "Vous devez spécifier votre mot de passe actuel";
					return false;
				}
				if (!e.target[1].value) {
					changeStatus = "Vous devez spécifier un nouveau mot de passe pour changer votre mot de passe";
					return false;
				}
				if (e.target[1].value !== e.target[2].value) {
					changeStatus = "Les nouveaux mots de passe ne correspondent pas";
					return false;
				}

				e.target[4].disabled = true;

				await clm.meowerRequest({cmd: "direct", val: {cmd: "change_pswd", val: e.target[1].value}});

				$modalShown = false;

				localStorage.clear();

				screen.set("setup");
				await tick();
				setupPage.set("reconnect");
			}}
		>
			{#if changeStatus}
				<label for="old-password-input" style="color: red;">{changeStatus}</label>
			{/if}
			<input id="old-password-input" type="password" class="modal-input white" placeholder="Mot de passe actuel" maxlength="64" /><br /><br />
			<input id="new-password-input" type="password" class="modal-input white" placeholder="Nouveau mot de passe" maxlength="64" /><br /><br />
			<input type="password" class="modal-input white" placeholder="Confirmer le nouveau mot de passe" maxlength="64" /><br /><br />
			<div class="modal-buttons">
				<button type="button" on:click={() => {$modalShown = false}}>Annuler</button>
				<button type="submit">Changer le mot de passe</button>
			</div>
		</form>
	</div>
</Modal>
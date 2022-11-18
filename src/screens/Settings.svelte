<!-- You probably know what this is. -->

<script>
	import Container from "../lib/Container.svelte";

	import {user, modalShown, modalPage} from "../lib/stores.js";
	import * as clm from "../lib/clmanager.js";
</script>

<!--
	<p>Quote: {$user.quote}</p>
	<form 
		class="createpost"
		on:submit|preventDefault={e => {
			//spinner.set(true);
			const _user = $user;
			_user.quote = e.target[0].value;
			user.set(_user);

			clm.updateProfile();
			//spinner.set(false);
		}}
	>
		<input
			type="text"
			class="white"
			placeholder="Write something..."
				id="qinput"
				name="qinput"
			autocomplete="false"
		>
		<button>Save Quote</button>
	</form>
-->
<Container>
	<h1>Réglages</h1>
	Vous pouvez modifier vos paramètres ici. Ceux-ci seront enregistrés sur votre compte, de sorte qu'ils seront transférés à d'autres clients.
</Container>
<Container>
	<div class="settings-controls">
		<button
			class="circle settings"
			on:click={()=>{
				const _user = $user;
				_user.layout = _user.layout === "new" ? "old" : "new";
				user.set(_user);

				clm.updateProfile();
			}}
		></button>
	</div>

	<h2>Disposition</h2>
	{#if $user.layout === "new"}
			<p>Le thème est actuellement défini sur <b>nouveau</b></p>
			{:else}
			<p>Le thème est actuellement défini sur <b>ancien</b></p>
			{/if}
</Container>
<Container>
	<div class="settings-controls">
		<button
			class="circle settings"
			on:click={()=>{
				const _user = $user;
				_user.theme = _user.theme === "orange" ? "blue" : "orange";
				user.set(_user);

				clm.updateProfile();
			}}
		></button>
	</div>

	<h2>Thème</h2>
	{#if $user.theme === "orange"}
			<p>Le thème est actuellement défini sur <b>orange</b></p>
			{:else}
			<p>Le thème est actuellement défini sur <b>bleu</b></p>
			{/if}
</Container>
<Container>
	<div class="settings-controls">
		<input
			type="checkbox"
			checked={!$user.mode}
			on:change={()=>{
				const _user = $user;
				_user.mode = !_user.mode;
				user.set(_user);

				clm.updateProfile();
			}}
		>
	</div>

	<h2>Mode sombre</h2>
	Le mode sombre est actuellement <b>{$user.mode ? "désactivé" : "activé"}</b>.
</Container>
<Container>
	<div class="settings-controls">
		<input
			type="checkbox"
			checked={$user.sfx}
			on:change={()=>{
				const _user = $user;
				_user.sfx = !_user.sfx;
				user.set(_user);

				clm.updateProfile();
			}}
		>
	</div>

	<h2>Effets sonores</h2>
	Les effets sonores sont actuellement <b>{!$user.sfx ? "désactivé" : "activé"}</b>.
</Container>
{#if $user.name}
<Container>
	<div class="settings-controls">
		<button
			class="circle settings"
			on:click={() => {
				$modalPage = "changePassword";
				$modalShown = true;
			}}
		></button>
	</div>

	<h2>Changer le mot de passe</h2>
	Modifiez le mot de passe de votre compte.
</Container>
<Container>
	<div class="settings-controls">
		<button
			class="circle settings"
			on:click={() => {
				$modalPage = "deleteAccount";
				$modalShown = true;
			}}
		></button>
	</div>

	<h2>Supprimer le compte</h2>
	Supprimez définitivement votre compte Meower. ÇA NE PEUT PAS ÊTRE ANNULÉ.
</Container>
{/if}

<div class="eee"></div>

<style>
	.settings-controls {
		position: absolute;
		top: 0.25em;
		right: 0.25em;
	}

	input[type="checkbox"], button.circle {
		border: none;
		margin: 0;
		margin-left: 0.125em;
	}
</style>
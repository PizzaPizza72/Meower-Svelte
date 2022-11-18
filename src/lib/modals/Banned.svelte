<script>	
	import Modal from "../Modal.svelte";

	import {modalShown} from "../stores.js";
</script>

<Modal on:close={() => {$modalShown = false}}>
    <h2 slot="header">
        Compte banni
    </h2>
    <div slot="default">
        <p>
            Votre compte a été banni par l'équipe de modération de Meower car il a été utilisé pour violer les conditions d'utilisation de Meower.
            <br><br>
            Si vous souhaitez discuter de cette interdiction, veuillez envoyer un e-mail à <a href="mailto:support@meower.org" target="_blank">support@meower.org</a>.
        </p>
        <br>
        <div class="modal-buttons">
            <a href="https://meower.org/legal" target="_blank">Conditions d'utilisation</a>
            <button on:click={() => {$modalShown = false}}>Proche</button>
        </div>
    </div>
</Modal>
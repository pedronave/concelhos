<script>
    import Modal from './Modal.svelte';
</script>

<Modal on:close>
    <div class="modal__content">
        <div class="modal__section">
            <h2 class="modal__section--title">Como jogar</h2>
            <p>Neste jogo, todos os dias há uma nova imagem de um concelho.</p>
            <p>Tem 6 tentativas para descobrir o nome do concelho</p>
            <p>Se não acertar o nome do concelho, aparecerá a distância do concelho submetido ao concelho a descobrir, bem como a direção na qual o concelho se encontra.</p>
        </div>
    </div>
</Modal>


<style>
    .modal__section--title {
        text-align: center;
        margin: 0;
        margin-bottom: 1rem;
    }
</style>
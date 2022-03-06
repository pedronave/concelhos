<script>
	import { stats, timeToNext } from "./stores.js";
    import { clickOutside } from "./click_outside.js";
    import { createEventDispatcher } from 'svelte';
    import Modal from './Modal.svelte';

	const dispatch = createEventDispatcher();
</script>

<Modal on:close>
    <div class="modal__content">
        <div class="modal__section">
            <h2 class="modal__section--title">Estatísticas</h2>
            <div class="statistics-container">
                <div class="statistic">
                    <div>{$stats.played}</div>
                    <div>Jogados</div>
                </div>
                <div class="statistic">
                    <div>{$stats.played !== 0 ? Math.round($stats.wins / $stats.played * 100) : 0}</div>
                    <div>% Certos</div>
                </div>
            </div>
            
        </div>
    </div>

    <p>Próximo concelho está disponível em <b>{("0" + $timeToNext.getHours()).slice(-2)}h{("0" + $timeToNext.getMinutes()).slice(-2)}m{("0" + $timeToNext.getSeconds()).slice(-2)}s</b></p>
</Modal>
<!-- <div class="modal" use:clickOutside on:outclick={() => dispatch('close')}>
    <div class="modal__actions">
        <button on:click={() => dispatch('close')}>
            <svg xmlns="http://www.w3.org/2000/svg" height="16" width="16" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
              </svg>
        </button>
    </div>
    <div class="modal__content">
        <div class="modal__section">
            <h2 class="modal__section--title">Estatísticas</h2>
            <div class="statistics-container">
                <div class="statistic">
                    <div>{$stats.played}</div>
                    <div>Jogados</div>
                </div>
                <div class="statistic">
                    <div>{Math.round($stats.wins / $stats.played * 100)}</div>
                    <div>% Certos</div>
                </div>
            </div>
            
        </div>
    </div>

    <p>Próximo concelho está disponível em <b>{("0" + $timeToNext.getHours()).slice(-2)}h{("0" + $timeToNext.getMinutes()).slice(-2)}m{("0" + $timeToNext.getSeconds()).slice(-2)}s</b></p>
</div> -->

<style>
    .modal {
        background-color: rgb(233, 233, 233);
        padding: 2rem 2.5rem;
        border-radius: 8px;
        filter: drop-shadow(0 20px 13px rgb(0 0 0 / 0.03)) drop-shadow(0 8px 5px rgb(0 0 0 / 0.08));
    }

    .modal__actions {
        top: 0.5rem;
        right: 0.5rem;
        position: absolute;
        width: 100%;
        display: flex;
        justify-content: flex-end;
    }

    .modal__actions button {
        background-color: transparent;
        border: none;
        outline: none;
        cursor: pointer;
        width: 40px;
        height: 40px;
        margin: 0;
        padding: 0;
    }

    .modal__section--title {
        text-align: center;
        margin: 0;
        margin-bottom: 1rem;
    }

    .statistics-container {
        display: flex;
        justify-content: center;
        gap: 2rem;
    }

    .statistic {
        display: flex;
        flex-direction: column;
        align-items: center;
    }
</style>
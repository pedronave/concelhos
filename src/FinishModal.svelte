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

    <p>Próximo concelho está disponível em <b>{("0" + (Math.min(0, $timeToNext.getHours() - 1))).slice(-2)}h{("0" + $timeToNext.getMinutes()).slice(-2)}m{("0" + $timeToNext.getSeconds()).slice(-2)}s</b></p>
</Modal>

<style>
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
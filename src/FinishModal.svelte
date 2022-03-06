<script>
	import { stats, timeToNext } from "./stores.js";
</script>

<div class="modal">
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
</div>

<style>
    .modal {
        background-color: rgb(233, 233, 233);
        padding: 2rem 2.5rem;
        border-radius: 8px;
        filter: drop-shadow(0 20px 13px rgb(0 0 0 / 0.03)) drop-shadow(0 8px 5px rgb(0 0 0 / 0.08));
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
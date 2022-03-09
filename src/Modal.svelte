<script>
	import { createEventDispatcher, onDestroy } from 'svelte';

	const dispatch = createEventDispatcher();
	const close = () => dispatch('close');

	let modal;

	const handle_keydown = e => {
		if (e.key === 'Escape') {
			close();
			return;
		}

		if (e.key === 'Tab') {
			// trap focus
			const nodes = modal.querySelectorAll('*');
			const tabbable = Array.from(nodes).filter(n => n.tabIndex >= 0);

			let index = tabbable.indexOf(document.activeElement);
			if (index === -1 && e.shiftKey) index = 0;

			index += tabbable.length + (e.shiftKey ? -1 : 1);
			index %= tabbable.length;

			tabbable[index].focus();
			e.preventDefault();
		}
	};

	const previously_focused = typeof document !== 'undefined' && document.activeElement;

	if (previously_focused) {
		onDestroy(() => {
			previously_focused.focus();
		});
	}
</script>

<svelte:window on:keydown={handle_keydown}/>

<div class="modal-background" on:click={close}></div>

<div class="modal" role="dialog" aria-modal="true" bind:this={modal}>
    <div class="modal__actions">
        <button autofocus on:click={() => dispatch('close')}>
            <svg xmlns="http://www.w3.org/2000/svg" height="16" width="16" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
              </svg>
        </button>
    </div>
	<slot></slot>
</div>

<style>
	.modal-background {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: rgba(0,0,0,0.3);
	}

	.modal {
		position: absolute;
		left: 50%;
		top: 50%;
		width: calc(100vw - 4em);
		max-width: 32em;
		max-height: calc(100vh - 4em);
		overflow: auto;
		transform: translate(-50%,-50%);
		padding: 1em;
		border-radius: 0.2em;
		background: white;
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
</style>

<!-- <style>
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
</style> -->
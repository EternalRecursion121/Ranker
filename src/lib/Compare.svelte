<script>
    import { createEventDispatcher } from 'svelte';

    export let a;
    export let b;

    let result = '';

    const dispatch = createEventDispatcher();

    function compare(option) {
        result = option === a;
        dispatch('result', result);
    }
</script>

<main class="flex justify-center items-center h-screen">
    <div class="flex flex-row items-center">
        <button class="bg-indigo-600 text-white px-8 py-4 rounded m-5 text-2xl" on:click={() => compare(a)}>{a}</button>
        <button class="bg-indigo-600 text-white px-8 py-4 rounded m-5 text-2xl" on:click={() => compare(b)}>{b}</button>
    </div>
</main>
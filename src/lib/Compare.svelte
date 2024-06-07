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

    document.addEventListener('keydown', (event) => {
        if (event.key === '1') {
            compare(a);
        } else if (event.key === '2') {
            compare(b);
        }
    });
</script>

<main class="flex justify-center items-center h-screen">
    <div class="flex flex-row items-center">
        <button class="bg-indigo-600 hover:indigo-700 hover:scale-110 transition duration-100 ease-in-out text-white px-8 py-4 rounded m-5 text-2xl" on:click={() => compare(a)}>{a}</button>
        <button class="bg-indigo-600 hover:indigo-700 hover:scale-110 transition duration-100 ease-in-out text-white px-8 py-4 rounded m-5 text-2xl" on:click={() => compare(b)}>{b}</button>
    </div>
</main>
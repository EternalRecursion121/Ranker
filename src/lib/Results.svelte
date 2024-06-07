<script>
    import { dndzone } from 'svelte-dnd-action';
    import {flip} from 'svelte/animate';
	const flipDurationMs = 200;

    export let results;

    let items = results.map((item, index) => ({id: index, title: item}));
    console.log(items);


    // Function to update the results on drag end
    function handleDndUpdate(event) {
        items = event.detail.items;
    }
</script>

<style>
    .list-item {
        padding: 10px;
        margin: 5px;
        background-color: #f3f4f6;
        border-radius: 4px;
        border: 1px solid #d1d5db;
        cursor: grab;
    }
    .list-item:hover {
        background-color: #e5e7eb;
    }
</style>

<h1 class="text-4xl font-bold mb-1 text-indigo-800">Results</h1>
<h2 class="text-sm text-gray-800 mb-4">(Drag and drop to adjust)</h2>
{#if items.length > 0}
    <section use:dndzone={{ items, flipDurationMs, dropTargetStyle:{outline:'none'} }} on:consider={handleDndUpdate} on:finalize={handleDndUpdate}>
        {#each items as item, index (item.id)}
            <div animate:flip={{duration:flipDurationMs}} class="list-item list-none text-gray-900">
                <b class="text-indigo-700">{index + 1}.</b> {item.title}
            </div>
        {/each}
    </section>
{:else}
    <p>No results to display.</p>
{/if}

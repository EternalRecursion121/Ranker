<script>
    import { createEventDispatcher } from "svelte";

    export let items = [];
    const dispatch = createEventDispatcher();
    let newItem = "";

    function addNewItem() {
        if (newItem.trim() !== "") {
            items = [...items, newItem];
            newItem = "";
        }
    }

    function removeItem(index) {
        items = items.filter((_, i) => i !== index);
    }
</script>

<div class="container mx-auto">
    <h1 class="text-2xl font-bold mb-4">Ranker</h1>
    <div class="flex mb-4">
        <input
            type="text"
            class="flex-grow border border-gray-300 rounded-l py-2 px-4 mr-2"
            placeholder="Enter an item"
            bind:value={newItem}
            on:keydown={(event) => {
                if (event.key === "Enter") {
                    addNewItem();
                }
            }}
        />
        <button
            class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-r"
            on:click={addNewItem}
        >
            Add
        </button>
    </div>

    {#if items.length > 0}
        <ul class="border border-gray-300 rounded p-4">
            {#each items as item, index}
                <li class="flex items-center justify-between py-2 border-b border-gray-200">
                    <span>{item}</span>
                    <button
                        class="text-red-500 hover:text-red-600"
                        on:click={() => removeItem(index)}
                    >
                        Remove
                    </button>
                </li>
            {/each}
        </ul>
    {:else}
        <p class="text-gray-500">No items added yet.</p>
    {/if}
    <div class="flex justify-center mt-4">
        <button
        class="bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-2 px-4 rounded"
        on:click={() => items.length > 0 && dispatch("start-ranking")}
        >
        Rank Items
        </button>
    </div>
</div>

<style>
    .container {
        max-width: 600px;
        margin-left: auto;
        margin-right: auto;
    }
</style>
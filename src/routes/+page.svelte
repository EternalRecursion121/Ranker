<script>
    import Entry from "$lib/Entry.svelte";
    import Compare from "$lib/Compare.svelte";

    let items = [];
    let rankingStarted = false;
    let rankingFinished = false;
    let comparer;
    let a, b;

    async function rank(a) {
        console.log(1, a);
        let median_pos = Math.floor(a.length / 2);
        let l, r;
        l, r = a.slice(0, median_pos), a.slice(median_pos, a.length);

        l = await rank(l);
        r = await rank(r);

        return merge(l, r);
    }

    async function merge(l, r) {
        let result = [];
        let i = 0;
        let j = 0;

        while (i < l.length && j < r.length) {
            if (await compare(l[i], r[j])) {
                result.push(l[i]);
                i++;
            } else {
                result.push(r[j]);
                j++;
            }
        }

        while (i < l.length) {
            result.push(l[i]);
            i++;
        }

        while (j < r.length) {
            result.push(r[j]);
            j++;
        }

        return result;
    }

    async function compare(c1, c2) {
        let a = c1;
        let b = c2;
        return new Promise((resolve) => {
            let result = false;
            // Event listener for comparison result
            const comparisonResultHandler = (event) => {
                console.log("triggered");
                const { detail } = event;
                result = detail;
                resolve(result);
            };
            // Add event listener
            comparer.addEventListener('comparisonResult', comparisonResultHandler);
        });
    }

    function startRanking() {
        rankingStarted = true;
        let rankedItems = rank(items);
    }


</script>

<main class="p-4">
    {#if !rankingStarted}
        <Entry bind:items={items} on:start-ranking={startRanking()}/>
    {:else}
        <Compare {a} {b} bind:this={comparer} on:comparisonResult={({detail}) => console.log(detail)} />
    {/if}
</main>


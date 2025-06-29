<script>
    import { flip } from "svelte/animate";
    import { dndzone } from "svelte-dnd-action";
    import { createEventDispatcher } from "svelte";

    export let items = [];
    const dispatch = createEventDispatcher();
    const flipDurationMs = 300;

    let damageMap = {};

    function handleDndConsider(e) {
        items = e.detail.items;
    }

    function handleDndFinalize(e) {
        items = e.detail.items;
        dispatch('update', items);
    }

    function applyDamage(id, damage = 0) {
        const parsed = parseInt(damage);
        if (isNaN(parsed)) return;

        items = items.map(item =>
            item.id === id ? { ...item, health: item.health - parsed } : item
        );
        dispatch('update', items);

        damageMap[id] = 0;
    }
</script>

<style>
    section {
        padding: 0.3em;
        border: 1px solid #e40712;
        min-height: 90vh;
        margin: 0 auto;
        width: 77%;
        background-image: url("background.png");
        background-repeat: repeat;
    }
    div {
        width: 75rem;
        padding: 0.2em;
        margin: 0.15em 0;
        background-image: url("list.png");
        background-repeat: no-repeat;
        display: flex;
        align-items: center;
        justify-content: space-between;

    }
    .charater-health{
        color: #35B000;
    }
    .button-damage{
        margin-right: 10px ;
        max-width: 255px;
        background: black;
        color: white;
        border: 2px solid #bc0f0f;
        border-radius: 3px;
        width: auto ;
        padding: 8px 16px;
    }

</style>

<section use:dndzone={{ items, flipDurationMs }} on:consider={handleDndConsider} on:finalize={handleDndFinalize}>
    {#each items as item (item.id)}
        <div animate:flip={{ duration: flipDurationMs }}>
            <img src={`/img/${item.type || 'default'}.jpg`} alt={item.type} width="64" height="64" />
            <output><strong>{item.name}</strong></output>
            <output>Vida: <strong class="charater-health">{item.health}</strong></output>
            <input 
                type="number" 
                min="0"
                bind:value={damageMap[item.id]} 
                placeholder="Dano"
               
            />
            <button class="button-damage" on:click={() => applyDamage(item.id, damageMap[item.id] || 0)}>Aplicar dano</button>
        </div>
    {/each}
</section>
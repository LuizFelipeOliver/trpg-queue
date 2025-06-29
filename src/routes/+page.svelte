<script lang="ts">
    import DnDList from '$lib/components/DnDList.svelte';

    type EntityUser =  ''| 'player' | 'monster';

    interface Character {
        id: number;
        name: string;
        health: number;
        type: EntityUser;
    };

    let character: Omit<Character, 'id'> = {
        name: '',
        health: 0,
        type:''
    };

    let queue: Character[] = [];
    let nextId = 0;

    function handleAdd(event: Event) {
        event.preventDefault();

        if (!character.name || !character.type) {
            alert("Preencha todos os campos");
            return;
    
        }

        queue = [...queue, { ...character, id: nextId++ }];
        
        character = { name: '', health: 0, type: '' };
        console.log(queue);
    }

</script>

<style>
    form{
        margin-bottom: 5px;
    }

</style>

<h1>Gerenciador de RPG</h1>

<form on:submit={handleAdd}>
    
    <select bind:value={character.type}>
        <option value="">Selecione o tipo</option>
        <option value="player">Jogador</option>
        <option value="monster">Monstro</option>
    </select>

    <input 
        type="text" 
        name="character-name"
        placeholder="Nome"
        id="character-name" 
        bind:value={character.name}
    />

    <input 
        type="number"
        placeholder="Vida" 
        name="character-health" 
        id="character-health" 
        bind:value={character.health}
    />

    <button type="submit">adicionar</button>
</form>


<DnDList items={queue} on:update={(e) => queue = e.detail} />

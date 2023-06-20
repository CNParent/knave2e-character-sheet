<script>
    import Item from "./Item.svelte";
    import ListItem from "./ListItem.svelte";
    import listActions from "../lib/listActions.js";

    export let model;

    function addEquipment() {
        model.equipment.push({ name: '', size: 1 });
        model.equipment = model.equipment;
    }

    function moveEquipment(n, item) {
        listActions.move(model.equipment, n, item);
        model.equipment = model.equipment;
    }

    function removeEquipment(item) {
        listActions.remove(model.equipment, item);
        model.equipment = model.equipment;
    }

    $:capacity = 10 + model.abilities.constitution;
    $:used = model.equipment.reduce((a,b) => a + b.size, 0);
    $:btnStyle = used > capacity ? 'btn-danger' : 'btn-dark';
</script>

<div class="d-flex align-items-end m-1">
    <button on:click={addEquipment} class="btn btn-dark">Add</button>
    <span class="ml-auto btn {btnStyle}">{used}/{capacity}</span>
</div>
{#each model.equipment as item}
    <ListItem item={item} move={moveEquipment} remove={removeEquipment}>
        <Item bind:item={item}></Item>
    </ListItem>
{/each}

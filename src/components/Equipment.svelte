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

    function addWound() {
        model.wounds.push({ name: '', size: 1 });
        model.wounds = model.wounds;
    }

    function moveWound(n, item) {
        listActions.move(model.wounds, n, item);
        model.wounds = model.wounds;
    }

    function removeWound(item) {
        listActions.remove(model.wounds, item);
        model.wounds = model.wounds;
    }

    $:totalItemSize = model.equipment.reduce((a,b) => a + b.size, 0);
    $:totalWoundSize = model.wounds.reduce((a,b) => a + b.size, 0);
    $:maxWoundSize = 10 + model.abilities.constitution;
    $:maxItemSize = 10 + model.abilities.constitution - totalWoundSize;

    $:itemBtnStyle = totalItemSize > maxItemSize ? 'btn-danger' :
        totalWoundSize > 0 ? 'btn-warning' : 
        'btn-dark';

    $:woundBtnStyle = totalWoundSize >= maxWoundSize ? 'btn-danger' :
        totalWoundSize > 0 ? 'btn-warning' :
        'btn-dark';

</script>
<div class="d-flex align-items-end m-1">
    <span>Equipment</span>
    <span title="capacity" class="ml-auto btn {itemBtnStyle}">{totalItemSize}/{maxItemSize}</span>
    <button on:click={addEquipment} class="ml-1 btn btn-dark">Add</button>
</div>
{#each model.equipment as item}
    <ListItem item={item} move={moveEquipment} remove={removeEquipment}>
        <Item bind:item={item}></Item>
    </ListItem>
{/each}
<hr />
<div class="d-flex align-items-end m-1">
    <span>Wounds</span>
    <span title="capacity" class="ml-auto btn {woundBtnStyle}">{totalWoundSize}/{maxWoundSize}</span>
    <button on:click={addWound} class="ml-1 btn btn-dark">Add</button>
</div>
{#each model.wounds as wound}
    <ListItem item={wound} move={moveWound} remove={removeWound}>
        <Item bind:item={wound} btnStyle="btn-danger"></Item>
    </ListItem>
{/each}

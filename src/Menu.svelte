<script>
    import { onMount } from "svelte/internal";
    import MenuElement from "./MenuElement.svelte";

    export let menu_items;
</script>

<ul class="menu">
    {#each menu_items as menu_item}
        <MenuElement {menu_item} />
    {/each}
</ul>

<style>
    ul {
        user-select: none;
        list-style: none;
        font-family: sans-serif;
        font-size: 14px;
    }
</style>

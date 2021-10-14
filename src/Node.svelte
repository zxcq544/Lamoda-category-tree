<script>
    // import { onMount } from "svelte";
    export let children = [];
    export let name = "";
    export let indent = 0;
    // let wrapper;
    // let menu;
    let is_open = true;
    function toggle_open() {
        is_open = !is_open;
    }
</script>

<ul style="padding-left: {indent}px">
    <span on:click={toggle_open}>
        {name}
    </span>
    {#if is_open}
        {#if typeof children !== "undefined" && children.length > 0}
            <div class="menu">
                <div class="wrapper">
                    <li class="child_el">
                        {#each children as child}
                            <svelte:self {...child} indent={indent + 24} />
                        {/each}
                    </li>
                </div>
            </div>
        {/if}
    {/if}
</ul>

<style>
    .menu {
        /* height: 0; */
        overflow: hidden;
        transition: height 0.4s ease-in;
    }
    ul {
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
        align-content: flex-start;
        justify-content: flex-start;
        align-items: stretch;
        width: fit-content;
    }
    li {
        padding: 5px;
    }
    span {
        cursor: pointer;
    }
</style>

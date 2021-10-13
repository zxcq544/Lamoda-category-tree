<script>
    import { onMount } from "svelte";
    let menu;
    let wrapper;
    let size;
    onMount(() => {
        size = wrapper.clientHeight;
    });

    function grow() {
        if (menu.clientHeight != 0) {
            menu.style.height = 0;
        } else {
            menu.style.height = size + "px";
        }
    }
</script>

<ul>
    <span on:click={grow}>Dropdown menu</span>
    <div class="menu" bind:this={menu} style="border: pink solid 1px;">
        <div class="wrapper" bind:this={wrapper}>
            <li>one</li>
            <li>two</li>
            <li>three</li>
        </div>
    </div>
</ul>

<style>
    .menu {
        height: 0;
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

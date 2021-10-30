<script>
    import { onMount } from "svelte";

    export let menu;
    let menu_items_height = {};
    let menu_items_height_original;
    let menu_items_is_open = {};

    onMount(() => {
        for (let i = 0; i < menu.length; i++) {
            menu_items_height["id" + "-" + i] = document.getElementById("id" + "-" + i).clientHeight;
            menu_items_is_open["id" + "-" + i] = true;
            for (let j = 0; j < menu[i].children.length; j++) {
                menu_items_height["id" + "-" + i + "-" + j] = document.getElementById(
                    "id" + "-" + i + "-" + j
                ).clientHeight;
                menu_items_is_open["id" + "-" + i + "-" + j] = true;
            }
        }
        //make copy of menu_items_height for use as element height when open
        menu_items_height_original = Object.assign({}, menu_items_height);
        // console.log(menu);
        // console.log(menu_items_height_original);
        console.log(menu_items_is_open);
    });
    function collapse_top_level(ev) {
        let index = ev.target.dataset.index;
        if (menu_items_height[index] == 0) {
            menu_items_height[index] = menu_items_height_original[index];
            menu_items_is_open[index] = true;
            console.log(menu_items_is_open);
        } else {
            menu_items_height[index] = 0;
            menu_items_is_open[index] = false;
            console.log(menu_items_is_open);
        }
    }
    function collapse_second_level(ev) {
        let index = ev.target.dataset.index;
        let index_top_level = ev.target.dataset.parent_index;
        if (menu_items_height[index] == 0) {
            menu_items_height[index] = menu_items_height_original[index];
            menu_items_is_open[index] = true;
            console.log(menu_items_is_open);
            menu_items_height[index_top_level] += menu_items_height_original[index];
            menu_items_height_original[index_top_level] = menu_items_height[index_top_level];
        } else {
            menu_items_height[index] = 0;
            menu_items_is_open[index] = false;
            console.log(menu_items_is_open);
            menu_items_height[index_top_level] -= menu_items_height_original[index];
            menu_items_height_original[index_top_level] = menu_items_height[index_top_level];
        }
    }
</script>

<ul>
    {#each menu as menu_top_level, menu_top_level_index}
        <li>
            <div class="menu-item-with-arrow">
                <span data-index="id-{menu_top_level_index}" on:click={collapse_top_level}>{menu_top_level.name}</span>
                <span class="arrow_closed" class:arrow={menu_items_is_open["id-" + menu_top_level_index] === true} />
            </div>
            <ul id="id-{menu_top_level_index}" style="height:{menu_items_height['id' + '-' + menu_top_level_index]}px">
                {#each menu_top_level.children as menu_second_level, menu_second_level_index}
                    <li>
                        <span
                            data-index="id-{menu_top_level_index}-{menu_second_level_index}"
                            data-parent_index="id-{menu_top_level_index}"
                            on:click={collapse_second_level}>{menu_second_level.name}</span
                        >
                        <ul
                            id="id-{menu_top_level_index}-{menu_second_level_index}"
                            style="height:{menu_items_height[
                                'id' + '-' + menu_top_level_index + '-' + menu_second_level_index
                            ]}px"
                        >
                            {#each menu_second_level.children as menu_third_level, menu_third_level_index}
                                <li id="id-{menu_top_level_index}-{menu_second_level_index}-{menu_third_level_index}">
                                    <span>{menu_third_level.name}</span>
                                </li>
                            {/each}
                        </ul>
                    </li>
                {/each}
            </ul>
        </li>
    {/each}
</ul>

<style>
    ul {
        overflow: hidden;
        transition: height linear 0.3s;
        user-select: none;
        list-style: none;
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
        width: fit-content;
    }
    li {
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
    }
    span {
        display: flex;
        font-family: sans-serif;
        cursor: pointer;
    }
    .menu-item-with-arrow:hover .arrow_closed {
        width: 24px;
        background: url(/i/filter-arrow.svg) center center no-repeat;
    }
    .arrow {
        transform: rotate(180deg);
        width: 24px;
        background: url(/i/filter-arrow.svg) center center no-repeat;
    }
    .menu-item-with-arrow {
        display: flex;
        flex-direction: row;
        flex-wrap: nowrap;
        justify-content: space-between;
    }
</style>

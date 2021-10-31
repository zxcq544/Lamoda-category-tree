<script>
    import { onMount } from "svelte";

    export let menu;

    onMount(() => {
        for (let i = 0; i < menu.length; i++) {
            menu[i].height = document.getElementById("id" + "-" + i).clientHeight;
            menu[i].previous_height = menu[i].height;
            menu[i].is_open = true;
            for (let j = 0; j < menu[i].children.length; j++) {
                menu[i].children[j].height = document.getElementById("id-" + i + "-" + j).clientHeight;
                menu[i].children[j].previous_height = menu[i].children[j].height;
                menu[i].children[j].is_open = true;
            }
        }
        console.log(menu);
    });
    function collapse_top_level(ev) {
        let i = ev.target.dataset.index;
        if (menu[i].height == 0) {
            menu[i].height = menu[i].previous_height;
            menu[i].is_open = true;
        } else {
            menu[i].height = 0;
            menu[i].is_open = false;
        }
    }
    function collapse_second_level(ev) {
        let i = ev.target.dataset.index;
        let i_parent = ev.target.dataset.parent_index;
        if (menu[i_parent].children[i].height == 0) {
            menu[i_parent].children[i].height = menu[i_parent].children[i].previous_height;
            menu[i_parent].children[i].is_open = true;
            menu[i_parent].height += menu[i_parent].children[i].previous_height;
            menu[i_parent].previous_height = menu[i_parent].height;
        } else {
            menu[i_parent].children[i].height = 0;
            menu[i_parent].children[i].is_open = false;
            menu[i_parent].height -= menu[i_parent].children[i].previous_height;
            menu[i_parent].previous_height = menu[i_parent].height;
        }
    }
</script>

<ul>
    {#each menu as menu_top_level, menu_top_level_index}
        <li>
            <div class="menu-item-with-arrow">
                <span data-index={menu_top_level_index} on:click={collapse_top_level}>{menu_top_level.name}</span>
                <span class="arrow_closed" class:arrow={menu[menu_top_level_index].is_open === true} />
            </div>
            <ul id="id-{menu_top_level_index}" style="height:{menu[menu_top_level_index].height}px">
                {#each menu_top_level.children as menu_second_level, menu_second_level_index}
                    <li>
                        <span
                            data-index={menu_second_level_index}
                            data-parent_index={menu_top_level_index}
                            on:click={collapse_second_level}>{menu_second_level.name}</span
                        >
                        <ul
                            id="id-{menu_top_level_index}-{menu_second_level_index}"
                            style="height:{menu[menu_top_level_index].children[menu_second_level_index].height}px"
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

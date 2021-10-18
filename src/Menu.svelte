<script>
    import { onMount } from "svelte";

    export let menu;
    let menu_items_height = {};
    let menu_items_height_original;

    onMount(() => {
        for (let i = 0; i < menu.length; i++) {
            menu_items_height["id" + i] = document.getElementById("id" + i).clientHeight;
            for (let j = 0; j < menu[i].children.length; j++) {
                menu_items_height["id" + i + "" + j] = document.getElementById("id" + i + "" + j).clientHeight;
            }
        }
        menu_items_height_original = JSON.parse(JSON.stringify(menu_items_height));
        // console.log(menu);
        // console.log(menu_items_height);
    });
    function collapse_top_level(ev) {
        let index = ev.target.dataset.index;
        if (menu_items_height[index] == 0) {
            menu_items_height[index] = menu_items_height_original[index];
        } else {
            menu_items_height[index] = 0;
        }
    }
    function collapse_second_level(ev) {
        let index = ev.target.dataset.index;
        let index_top_level = ev.target.dataset.index.slice(0, -1);
        if (menu_items_height[index] == 0) {
            menu_items_height[index] = menu_items_height_original[index];
            menu_items_height[index_top_level] += menu_items_height_original[index];
            menu_items_height_original[index_top_level] = menu_items_height[index_top_level];
        } else {
            menu_items_height[index] = 0;
            menu_items_height[index_top_level] -= menu_items_height_original[index];
            menu_items_height_original[index_top_level] = menu_items_height[index_top_level];
        }
    }
</script>

<ul>
    {#each menu as menu_top_level, menu_top_level_index}
        <li>
            <span data-index="id{menu_top_level_index}" on:click={collapse_top_level}>{menu_top_level.name}</span>
            <ul id="id{menu_top_level_index}" style="height:{menu_items_height['id' + menu_top_level_index]}px">
                {#each menu_top_level.children as menu_second_level, menu_second_level_index}
                    <li>
                        <span
                            data-index="id{menu_top_level_index}{menu_second_level_index}"
                            on:click={collapse_second_level}>{menu_second_level.name}</span
                        >
                        <ul
                            id="id{menu_top_level_index}{menu_second_level_index}"
                            style="height:{menu_items_height[
                                'id' + menu_top_level_index + '' + menu_second_level_index
                            ]}px"
                        >
                            {#each menu_second_level.children as menu_third_level, menu_third_level_index}
                                <li id="id{menu_top_level_index}{menu_second_level_index}{menu_third_level_index}">
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
        transition: height ease-in 0.3s;
        user-select: none;
        list-style: none;
    }
    span {
        font-family: sans-serif;
        cursor: pointer;
    }
</style>

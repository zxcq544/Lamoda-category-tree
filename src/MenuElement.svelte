<script>
    import { each, onMount } from "svelte/internal";

    export let menu_item;
    let first_child;
    let first_child_height;
    let first_child_height_original;

    let second_child_heights = [];
    let second_child_heights_original;

    let top_level_button;
    onMount(() => {
        first_child_height_original = first_child.clientHeight;
        first_child_height = first_child_height_original;

        for (let el = first_child.firstChild; el != null; el = el.nextSibling) {
            if (el.firstChild.nextElementSibling != null) {
                let in_li = el.firstChild.nextElementSibling;
                in_li.addEventListener("transitionend", function () {
                    in_li.previousElementSibling.removeAttribute("disabled");
                });
                second_child_heights.push(in_li.clientHeight);
                second_child_heights = second_child_heights;
            }
        }
        second_child_heights_original = JSON.parse(JSON.stringify(second_child_heights));

        first_child.addEventListener("transitionend", function () {
            top_level_button.removeAttribute("disabled");
        });
    });
    function first_child_collapse() {
        top_level_button.setAttribute("disabled", "disabled");
        if (first_child_height == 0) {
            first_child_height = first_child_height_original;
        } else {
            first_child_height = 0;
        }
    }
    function second_child_collapse(ev) {
        let ul = ev.target.nextElementSibling;
        if (ul == null) {
            return;
        }
        ev.target.setAttribute("disabled", "disabled");
        let original_height = second_child_heights_original[ul.dataset.index];

        if (ul.clientHeight == 0) {
            second_child_heights[ul.dataset.index] = original_height;
            first_child_height += original_height;
            first_child_height_original += original_height;
        } else {
            second_child_heights[ul.dataset.index] = 0;
            first_child_height -= original_height;
            first_child_height_original -= original_height;
        }
    }
</script>

<li>
    <button bind:this={top_level_button} on:click={first_child_collapse}>{menu_item.name}</button>
    <ul class="first_child" bind:this={first_child} style="height:{first_child_height}px">
        {#each menu_item["children"] as el, menu_index}
            <li>
                <button class="child_button" on:click={second_child_collapse}>{el.name}</button>
                {#if typeof el["children"] !== "undefined" && el["children"].length > 0}
                    <ul
                        class="second_child"
                        data-index={menu_index}
                        style="height:{second_child_heights[menu_index]}px"
                    >
                        {#each el["children"] as grandchild}
                            <li>
                                <span>{grandchild.name}</span>
                            </li>
                        {/each}
                    </ul>
                {/if}
            </li>
        {/each}
    </ul>
</li>

<style>
    ul {
        overflow: hidden;
        transition: height ease-in 0.3s;
        list-style: none;
        font-family: sans-serif;
        font-size: 14px;
        user-select: none;
    }
    button {
        font-size: 14px;
        font-family: sans-serif;
        background: none;
        border: none;
        cursor: pointer;
    }
</style>

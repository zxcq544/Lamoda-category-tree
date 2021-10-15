<script>
    import { each, onMount } from "svelte/internal";

    let menu_items = [
        {
            name: "First",
            children: [
                {
                    name: "Inner First",
                    children: [
                        {
                            name: "Inner Inner First",
                            children: [],
                        },
                        {
                            name: "Inner Inner Second",
                            children: [],
                        },
                    ],
                },
                {
                    name: "Inner Second",
                    children: [
                        {
                            name: "Inner Inner First",
                            children: [],
                        },
                        {
                            name: "Inner Inner Second",
                            children: [],
                        },
                    ],
                },
            ],
        },
        {
            name: "Second",
            children: [
                {
                    name: "Inner First",
                    children: [
                        {
                            name: "Inner Inner First",
                            children: [],
                        },
                        {
                            name: "Inner Inner Second",
                            children: [],
                        },
                    ],
                },
                {
                    name: "Inner Second",
                    children: [],
                },
                {
                    name: "Inner Third",
                    children: [],
                },
            ],
        },
        {
            name: "Third",
            children: [
                {
                    name: "Inner First",
                    children: [
                        {
                            name: "Inner Inner First",
                            children: [],
                        },
                        {
                            name: "Inner Inner Second",
                            children: [],
                        },
                    ],
                },
                {
                    name: "Inner Second",
                    children: [],
                },
                {
                    name: "Inner Third",
                    children: [],
                },
                {
                    name: "Inner Fourth",
                    children: [],
                },
            ],
        },
    ];
    let first_child_heights_array = [];
    let second_child_heights_array = {};

    let original_first_child_heights_array = [];
    let original_second_child_heights_array = {};
    onMount(() => {
        //Precalc height of elements and put into dictionary
        //TODO replace dict with array
        //TODO disable buttons during transition using transitionend
        let menu = document.getElementsByClassName("first_child");
        for (let i = 0; i < menu.length; i++) {
            let el = menu[i];
            first_child_heights_array.push(el.clientHeight);
            let inner_index = 0;
            for (let li = el.firstChild; li != null; li = li.nextSibling) {
                let second_child = li.firstChild.nextSibling.nextSibling;
                second_child_heights_array[i + "" + inner_index] = second_child.clientHeight;
                inner_index += 1;
            }
        }
        first_child_heights_array = first_child_heights_array;
        second_child_heights_array = second_child_heights_array;
        original_first_child_heights_array = JSON.parse(JSON.stringify(first_child_heights_array));
        original_second_child_heights_array = JSON.parse(JSON.stringify(second_child_heights_array));

        // console.log(first_child_heights_array);
        // console.log(second_child_heights_array);

        // console.log(original_first_child_heights_array);
        // console.log(original_second_child_heights_array);
    });
    function expand_first(ev) {
        let el = ev.target.nextElementSibling;
        // console.log(el);
        if (el.clientHeight == 0) {
            first_child_heights_array[ev.target.dataset.index] =
                original_first_child_heights_array[ev.target.dataset.index];
        } else {
            // el.style.height = 0 + "px";
            first_child_heights_array[ev.target.dataset.index] = 0;
        }
    }
    function expand_second(ev) {
        let el = ev.target.nextElementSibling;
        if (el.clientHeight == 0) {
            // el.style.height = second_child_heights_array[el.dataset.index] + "px";
            second_child_heights_array[el.dataset.index] = original_second_child_heights_array[el.dataset.index];
            first_child_heights_array[el.dataset.parent_index] += original_second_child_heights_array[el.dataset.index];
            // console.log(el.dataset.parent_index);
        } else {
            // el.style.height = 0 + "px";
            second_child_heights_array[el.dataset.index] = 0;
            first_child_heights_array[el.dataset.parent_index] -= original_second_child_heights_array[el.dataset.index];
        }
    }
</script>

<ul id="menu">
    {#each menu_items as menu_item, menu_index}
        <li>
            <button class="menu-span" on:click={expand_first} data-index={menu_index}>
                {menu_item.name}
            </button>
            <ul
                class="first_child"
                style="transition: height 0.4s ease-in; 
                height:{first_child_heights_array[menu_index]}px"
            >
                {#each menu_item.children as child_el, menu_child_index}
                    <li>
                        <button on:click={expand_second}>
                            {child_el.name}
                        </button>
                        <ul
                            class="second_child"
                            style="transition: height 0.4s ease-in;
                            height:{second_child_heights_array[menu_index + '' + menu_child_index]}px"
                            data-index={menu_index + "" + menu_child_index}
                            data-parent_index={menu_index}
                        >
                            {#each child_el.children as grandchild_el}
                                <li>
                                    {grandchild_el.name}
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
        list-style: none;
        overflow: hidden;
        font-family: sans-serif;
    }
    button {
        cursor: pointer;
        background: none;
        border: none;
    }
</style>

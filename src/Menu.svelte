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
    let first_child_height = 0;
    onMount(() => {
        let el = document.getElementById("first_child");
        console.log(window.getComputedStyle(el).height);
        first_child_height = el.clientHeight;
        console.log(window.getComputedStyle(el).height);
        el.style.height = 0;
    });
    function expand_first() {
        let el = document.getElementById("first_child");
        if (el.dataset.is_open == 1) {
            let when_done2 = setInterval(function () {
                if (el.clientHeight > 0) {
                    el.style.height = el.clientHeight - 2 + "px";
                    console.log(el.clientHeight);
                } else {
                    clearInterval(when_done2);
                    console.log(when_done2);
                    el.dataset.is_open = 0;
                    console.log("is open", el.dataset.is_open);
                }
            }, 10);
        } else {
            console.log("first_child_height", first_child_height);
            let when_done = setInterval(function () {
                if (el.clientHeight < first_child_height) {
                    el.style.height = el.clientHeight + 2 + "px";
                } else {
                    clearInterval(when_done);
                    console.log(when_done);
                    el.dataset.is_open = 1;
                    console.log("is open", el.dataset.is_open);
                }
            }, 10);
        }
    }
</script>

<ul>
    {#each menu_items as menu_item}
        <li>
            <span on:click={expand_first}>
                {menu_item.name}
            </span>
            <ul id="first_child" data-is_open="0">
                {#each menu_item.children as child_el}
                    <li>
                        {child_el.name}
                        <ul>
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
    }
</style>

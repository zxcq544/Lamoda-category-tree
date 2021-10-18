<script>
    import Menu from "./Menu.svelte";
    let menu = [
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
                        {
                            name: "Inner Inner Third",
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
                    name: "Inner Third",
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
            name: "Third",
            children: [
                {
                    name: "Inner First",
                    children: [],
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
</script>

<Menu {menu} />

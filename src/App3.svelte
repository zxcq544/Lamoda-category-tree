<script>
    import Node from "./Node.svelte";
    let root = {
        name: "Root",
        children: [
            {
                name: "Child 1",
                children: [
                    {
                        name: "Grandchild 1",
                        children: [],
                    },
                    {
                        name: "Grandchild 2",
                        children: [],
                    },
                    {
                        name: "Grandchild 3",
                        children: [],
                    },
                    {
                        name: "Grandchild 4",
                        children: [],
                    },
                ],
            },
            {
                name: "Child 2",
                children: [
                    {
                        name: "Grandchild 1",
                        children: [],
                    },
                    {
                        name: "Grandchild 2",
                        children: [],
                    },
                    {
                        name: "Grandchild 3",
                        children: [],
                    },
                ],
            },
            {
                name: "Child 3",
                children: [
                    {
                        name: "Grandchild 1",
                        children: [],
                    },
                    {
                        name: "Grandchild 2",
                        children: [],
                    },
                    {
                        name: "Grandchild 3",
                        children: [],
                    },
                ],
            },
        ],
    };
</script>

<Node {...root} />

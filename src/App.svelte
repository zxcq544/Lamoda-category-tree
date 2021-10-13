<script>
    //TODO: Each High level menu should be a component with low level menu items being a component as well.
    let menu = [
        {
            "Обувь": [
                {
                    "Ботинки": ["высокие ботинки", "низкие ботинки", "дезерты"],
                },
                {
                    "Кроссовки и кеды": ["кроссовки", "кеды"],
                },
            ],
        },
        {
            "Одежда": [],
        },
        {
            "Аксессуары": [],
        },
    ];
    import { onMount } from "svelte";

    let boots;
    let is_expanded = false;
    let height_of_menu = 10;
    let before_and_after_menu_calc = "initial";
    onMount(() => {
        height_of_menu = window.getComputedStyle(boots).height;
        console.log(boots);
        console.log(height_of_menu);
        console.log(before_and_after_menu_calc);
        before_and_after_menu_calc = 0;
        console.log(before_and_after_menu_calc);
    });
    function expand_menu() {
        is_expanded = !is_expanded;
    }
</script>

<div class="js-left-column-wrapper">
    <div class="table__left-column">
        <ul class="cat-nav">
            <li
                class="cat-nav-item to_hover"
                style="--height_of_menu:{height_of_menu}"
            >
                <span
                    class="cat-nav-item_a  cat-nav-item_root cat-nav-item_a_active"
                    on:click={expand_menu}
                >
                    <a class="link cat-nav_a" href="/">Обувь</a>
                </span>

                <ul
                    class="cat-nav cat-nav-sub to_expand"
                    bind:this={boots}
                    class:is_expanded
                    style="--before_and_after_menu_calc:{before_and_after_menu_calc}"
                >
                    <li class="cat-nav-item">
                        Ботинки

                        <ul class="cat-nav cat-nav-sub">
                            <li class="cat-nav-item">Высокие ботинки</li>
                            <li class="cat-nav-item">Низкие ботинки</li>
                            <li class="cat-nav-item">Дезерты</li>
                            <li class="cat-nav-item">Тимберленды и др.</li>
                            <li class="cat-nav-item">Трекинговые ботинки</li>
                            <li class="cat-nav-item">Челси</li>
                        </ul>
                    </li>
                    <li class="cat-nav-item">Домашняя обувь</li>
                    <li class="cat-nav-item">
                        Кроссовки и кеды
                        <ul class="cat-nav cat-nav-sub">
                            <li class="cat-nav-item">Кроссовки</li>
                            <li class="cat-nav-item">Кеды</li>
                            <li class="cat-nav-item">Бутсы</li>
                        </ul>
                    </li>
                    <li class="cat-nav-item">Мокасины и топсайдеры</li>
                    <li class="cat-nav-item">Резиновая обувь</li>
                    <li class="cat-nav-item">Сандалии</li>
                    <li class="cat-nav-item">Сапоги</li>
                </ul>
            </li>
            <li class="cat-nav-item">Одежда</li>
            <li class="cat-nav-item">Аксессуары</li>
            <li class="cat-nav-item">Premium</li>
        </ul>
    </div>
    <div class="table__right-column">
        <div class="product-catalog-main">
            <div class="js-multi-wrapper" />
        </div>
    </div>
    <div class="ease-out" />
</div>

<style>
    li {
        cursor: pointer;
    }
    .to_expand {
        overflow: hidden;
        max-height: var(--before_and_after_menu_calc);
        transition: max-height 0.7s ease-in;
    }
    .is_expanded {
        max-height: var(--height_of_menu);
    }
</style>

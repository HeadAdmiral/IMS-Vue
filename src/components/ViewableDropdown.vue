<template>
    <div class="dropdown" :id="id" @click="toggleDropdown">
        <a class="btn btn-default dropdown-toggle">
            <span class="dropdown-text">{{ selectedOption }}</span>
            <span class="caret"></span>
        </a>
        <ul class="dropdown-menu dropdown-menu-right" :class="{ 'hidden': isOpen === false }">
            <li v-for="(option, i) in options" :key="i" :class="{ 'is-active': i === selected }" v-on:mouseover="selected = i">
                <a>
                    {{ option }}
                </a>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        name: "viewable-dropdown",
        props: {
            options: {
                type: Array,
                required: true
            },
        },
        data() {
            return {
                isOpen: false,
                selected: -1,
                selectedOption: this.options[this.getRandomNumber(0, this.options.length)],
                id: 'dropdown-' + (new Date().getTime())
            }
        },
        methods: {
            toggleDropdown(e) {
                // Close dropdown
                this.isOpen = !this.isOpen;
                // Reset selected option
                this.selected = -1;

                if (e.target.text && e.target.text !== this.text) {
                    this.option = e.target.text.trim();

                    this.$root.$emit('changed', {
                        selected: e.target.text.trim(), // .trim() is necessary to remove surrounding whitespace from text
                        id: this.id
                    });
                }
            },
            handleClickOutside(e) {
                if (!this.$el.contains(e.target)) {
                    this.isOpen = false;
                }
            },
            getRandomNumber(min, max) {
                return Math.floor(Math.random() * (max - min)) + min;
            }
        },
        computed: {
            option: {
                get: function () {
                    return this.selectedOption;
                },

                set: function (newValue) {
                    this.selectedOption = newValue;
                }
            }
        },
        mounted() {
            this.$root.$emit('changed', {
                selected: this.selectedOption,
                id: this.id
            });

            document.addEventListener('click', this.handleClickOutside);
        },
        destroyed() {
            document.removeEventListener('click', this.handleClickOutside)
        },
    }
</script>

<style scoped>

    .dropdown {
        max-width: 126px;
        padding-bottom: 4px;
    }

    .btn {
        margin-left: 0;
    }

    .caret {
        display: inline-block;
        width: 0;
        height: 0;
        margin-left: 5px;
        vertical-align: middle;
        border-top: 4px solid;
        border-right: 4px solid transparent;
        border-left: 4px solid transparent;
    }

    .dropdown-menu {
        position: absolute;
        /*top: 100%;*/
        /*left: 0;*/
        z-index: 1000;
        min-width: 160px;
        padding: 5px 0;
        margin: 2px 0 0;
        list-style: none;
        font-size: 15px;
        text-align: left;
        background-color: #fff;
        border: 1px solid #ccc;
        border: 1px solid rgba(0,0,0,.15);
        border-radius: 2px;
        box-shadow: 0 6px 12px rgba(0,0,0,.175);
        background-clip: padding-box;
    }

    a {
        color: #333;
    }

    li>a {
        display: block;
        padding: 3px 20px;
        user-select: none;
    }

    .dropdown-menu>li>a:focus, .dropdown-menu>li>a:hover, .is-active>a {
        text-decoration: none;
        color: #fff;
        background-color: #2e8fda;
    }

    .hidden {
        display: none;
    }


</style>
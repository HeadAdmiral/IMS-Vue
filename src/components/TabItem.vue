<template>
    <div class="tabItem" :class="{ 'active': this.active, 'dd-tab': this.dropdown, 'open': this.dropdown && this.open, 'high': icon === 'reports', 'blocker': this.blocker }">
        <a @click="routeTo(route)">
            <li class="tab">
                <svg v-if="icon === 'dashboard'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path d="M422 496H304V384c0-8.8-7.2-16-16-16h-64c-8.8 0-16 7.2-16 16v112H90c-5.5 0-10-4.5-10-10V272H32.6c-5.5 0-10.8-2.6-13.8-7.2-4.3-6.5-3.2-14.9 2.1-20.1L244.7 21.6c6.2-6.2 16.3-6.2 22.6 0l223.9 223.1c4.6 4.6 6 11.5 3.5 17.4s-8.3 9.9-14.8 9.9H432v214c0 5.5-4.5 10-10 10zm-86-32h59c2.8 0 5-2.2 5-5V272c0-17.6 14.4-32 32-32h9.2L256 55.5 70.8 240H80c17.6 0 32 14.4 32 32v187c0 2.8 2.2 5 5 5h59v-80c0-26.5 21.5-48 48-48h64c26.5 0 48 21.5 48 48v80z"></path><circle cx="256" cy="224" r="32"></circle></svg>
                <svg v-if="icon === 'technicians'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path d="M256 318.8c39.6 0 77.7 11.9 110.1 34.4 31.7 22 55.9 52.6 69.9 88.4 3.1 8 7.6 19.8 10.1 30.1.8 3.4 1.3 6.2 1.6 8.3H64.3c.3-2.1.8-4.9 1.6-8.3 2.5-10.3 7-22.1 10.1-30.1 14-35.8 38.2-66.4 69.9-88.4 32.4-22.5 70.5-34.4 110.1-34.4m0-32c-95.4 0-177 59.4-209.8 143.2-15.4 39.4-27.5 82 14.8 82h390c42.3 0 30.2-42.6 14.8-82C433 346.2 351.4 286.8 256 286.8zm0-230.1h.7c45.7.4 83.2 37.9 83.6 83.6.2 22.7-8.5 44-24.4 60.1-16 16.1-37.2 25-59.9 25h-.7c-45.7-.4-83.2-37.9-83.6-83.6-.2-22.7 8.5-44 24.4-60.1 16-16.1 37.2-25 60.5-24.6M256 26c-63.8 0-115.5 52-115 116 .5 62.4 51.6 113.5 114 114h1c63.8 0 115.5-52 115-116-.5-62.4-51.6-113.5-114-114h-1z"></path></svg>
                <svg v-if="icon === 'storestock'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 25.7 25.7"><path d="M19.8 11.2l-6.3-7.7c-.4-.6-.8-.6-1.3 0L6 11.2H-.2v1.6h1.6l3.3 9.8H21l3.3-9.8h1.6v-1.6h-6.1zm-7-5.9l4.8 5.9H8.1l4.7-5.9zM19.9 21h-14l-2.7-8.1h19.5L19.9 21z"></path><path d="M8.3 16.2h9v1.1h-9z"></path></svg>
                <svg v-if="icon === 'buysale'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path d="M256 0c-53 0-96 43-96 96v64h32V96c0-35.3 28.7-64 64-64s64 28.7 64 64v64h32V96c0-53-43-96-96-96z"></path><path d="M416 160v320H96V160h320M96 128c-17.7 0-32 14.3-32 32v320c0 17.7 14.3 32 32 32h320c17.7 0 32-14.3 32-32V160c0-17.7-14.3-32-32-32H96z"></path><circle cx="176" cy="208" r="16"></circle><circle cx="336" cy="208" r="16"></circle></svg>
                <svg v-if="icon === 'returns'" xmlns="http://www.w3.org/2000/svg" viewBox="-192 -240 512 512"><path d="M-161.7 84.8c0 5.5 2.5 10.7 7.2 13.6l209.4 131c5.2 3.2 11.9 3.2 17.1-.1L281.7 98.4c4.7-2.9 7.3-8 7.3-13.5V-95.2c0-6.4-3.1-12.3-9-14.8L70.7-198.6c-4-1.7-8.4-1.7-12.4 0l-209.9 89.1c-5.8 2.4-10 8.2-10.1 14.5M64 6.5L-7.5-34.3l177-87.8 68.5 29L64 6.5zm.6-173l66 27.9L-41-53.5l-68.4-39.1 174-73.9zM-130-67.2l73 41.3V8.2c0 8.7 7.3 16 16 16s16-7.3 16-16V-7.6l73 41.9V187L-130 76V-67.2zM79 186.9V34.3L257-67.6V75.9l-178 111z"></path></svg>
                <svg v-if="icon === 'signmaker'" xmlns="http://www.w3.org/2000/svg" viewBox="-160 -208 512 512"><path d="M272 304c-4.3 0-8.4-1.7-11.5-4.9l-49.2-50.8-38.7 49.5c-2.8 3.6-7.1 5.8-11.6 6.1-4.6.3-9-1.4-12.3-4.7l-52.6-52.6-51.7 52.5c-3.2 3.3-7.7 5-12.3 4.7-4.6-.3-8.9-2.5-11.7-6.1l-38.8-49.6-50.2 51c-4.6 4.6-11.5 6-17.5 3.6-6-2.5-9.9-8.3-9.9-14.8V-176c0-17.6 14.4-32 32-32h320c17.6 0 32 14.4 32 32v464c0 6.5-4 12.4-10 14.8-1.9.8-4 1.2-6 1.2zM96 208c4.2 0 8.3 1.7 11.3 4.7l44.8 44.8c3.4 3.4 9 3.1 12-.7l33.3-42.6c2.8-3.6 7.1-5.9 11.8-6.1 4.6-.2 9.1 1.5 12.3 4.8l31.1 32.1c1.3 1.3 3.4.4 3.4-1.4V-160c0-8.8-7.2-16-16-16H-48c-8.8 0-16 7.2-16 16v404.1c0 1.8 2.2 2.7 3.4 1.4l32.2-32.7c3.2-3.3 7.7-5 12.3-4.7s8.9 2.5 11.7 6.1l33.2 42.5c3 3.8 8.6 4.1 12 .7l43.8-44.5c3-3.2 7.1-4.9 11.4-4.9z"></path><path d="M95-62.8C95-54.1 87.9-47 79.2-47H14.8C6.1-47-1-54.1-1-62.8v-.4C-1-71.9 6.1-79 14.8-79h64.4C87.9-79 95-71.9 95-63.2v.4zM191 32c0 9-7.1 16-15.8 16H14.8C6.1 48-1 40.9-1 32.2v-.3C-1 23.2 6.1 16 14.8 16h160.3c8.7 0 15.9 7.2 15.9 15.9v.1zm0 64c0 9-7.1 16-15.8 16H14.8C6.1 112-1 104.9-1 96.2v-.3C-1 87.2 6.1 80 14.8 80h160.3c8.7 0 15.9 7.2 15.9 16z"></path></svg>
                <svg v-if="icon === 'reports'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path d="M409.5 154l26.6 17.7-78 147-26.7-17.7zm-164.7 65.2l65 80-22.6 22.6-65-80zm-73.3 4.8l26.6 17.7L101.5 361l-26.6-17.7z"></path><path d="M67 354c16.6 0 30 13.4 30 30s-13.4 30-30 30-30-13.4-30-30 13.4-30 30-30m0-30c-33.1 0-60 26.9-60 60s26.9 60 60 60 60-26.9 60-60-26.9-60-60-60zm139-152c16.6 0 30 13.4 30 30s-13.4 30-30 30-30-13.4-30-30 13.4-30 30-30m0-30c-33.1 0-60 26.9-60 60s26.9 60 60 60 60-26.9 60-60-26.9-60-60-60zm118 168c16.6 0 30 13.4 30 30s-13.4 30-30 30-30-13.4-30-30 13.4-30 30-30m0-30c-33.1 0-60 26.9-60 60s26.9 60 60 60 60-26.9 60-60-26.9-60-60-60zM445 98c16.6 0 30 13.4 30 30s-13.4 30-30 30-30-13.4-30-30 13.4-30 30-30m0-30c-33.1 0-60 26.9-60 60s26.9 60 60 60 60-26.9 60-60-26.9-60-60-60z"></path></svg>
                {{ title }}
                <span :class="{ 'caret-right': open === false, 'caret-down': open }" v-if="dropdown" @click="routeTo(newRoute)"></span>
            </li>
        </a>
        <a class="new" v-if="newable" @click="routeTo(newRoute)">
            <svg xmlns="http://www.w3.org/2000/svg" width="512" height="512" viewBox="0 0 512 512" class="icon-xs"><path d="M511.5 227.5h-227V.5h-57v227H-.5v57h228v228h57v-228h227z"></path></svg>
        </a>

        <div class="dd" :class="{ 'open': this.dropdown && this.open }" v-if="dropdown">
            <a v-for="item in items" class="dropdown-item" :class="{ 'active': isActive(item.text) }" @click="routeTo(item.route)">
                <li >
                    {{ item.text }}
                </li>
            </a>
        </div>
    </div>
</template>

<script>
    import Arrow from '@/components/Arrow'
    export default {
        name: "tab-item",
        components: {
            Arrow
        },
        props: {
            title: String,
            active: Boolean,
            route: String,
            icon: String,
            newable: Boolean,
            newRoute: String,
            dropdown: Boolean,
            items: Array,
            blocker: Boolean,
        },
        data() {
            return {
                open: false
            }
        },
        methods: {
            routeTo(route) {
                if (route && !this.dropdown) {
                    let routeData = this.$router.resolve({path: route});
                    window.open(routeData.href, '_self');
                }
                else if (route && this.dropdown) {
                    let routeData = this.$router.resolve({path: route});
                    window.open(routeData.href, '_self');
                }
                else {
                    this.open = !this.open;
                }

            },
            isActive(val) {
                if (val.includes("Sign") && this.$route.name === "New Large Sign") {
                    return true;
                }

                return this.$route.name === val;
            }
        }
    }
</script>

<style scoped>
    * {
        user-select: none;
    }

    svg {
        margin-left: 3px;
        margin-right: 3px;
        height: 15px;
        width: 15px;
        fill: #d9d9d9;
        position: relative;
        bottom: -2px;
    }

    a {
        text-decoration: none;
        cursor: pointer;
        display: inline-block;
    }

    .active {
        background-color: #1f282d;
        border-left-color: #2FA3E6;
        color: #2FA3E6 !important;
        position: relative;
        z-index: 2;
    }

    .active svg {
        fill: #2FA3E6
    }

    .active a>li:nth-child(1) {
        color: #2FA3E6;
    }

    .tab {
        color: #d9d9d9;
        border-radius: 0;
        padding: 13px 30px 8px 15px;
        width: 150%;
    }

    .tabItem:not(.active) .tab {
        background-color: #2D373C;
    }

    .dropdown-item {
        color: #d9d9d9;
        border-radius: 0;
        padding: 13px 30px 8px 50px;
        width: 100%;
    }

    .tabItem.open .tab{
        background-color: #434f54;
        border-left-color: #434f54;
    }

    .tabItem:not(.active):not(.blocker):not(.dd-tab):hover a>li:nth-child(1), .tabItem.open .tab:hover, .dropdown-item:not(.active):hover {
        color: #fff;
        background-color: #2fa3e6;
        border-left-color: #2fa3e6;
    }

    .tabItem:not(.active) .tab:hover svg, .tabItem:not(.active):hover .new svg {
        fill: #fff!important;
    }

    .tabItem.dd-tab:not(.active) .tab:nth-child(1):hover {
        color: #fff;
    }

    .icon-xs:before {
        vertical-align: center;
    }

    .new {
        text-align: center;
        float: right;
        position: relative;
        top: 12px;
        right: 10px;
    }

    .tabItem:not(.active) .new svg {
        fill: none!important;
    }

    .icon-xs {
        height: 12px;
        width: 12px;
    }

    .caret-right {
        display: inline-block;
        position: relative;
        left: 61px;
        border: 4px solid #9FA29D;
        border-top: 4px solid transparent;
        border-right: 4px solid transparent;
        border-bottom: 4px solid transparent;
    }

    .caret-down {
        display: inline-block;
        position: relative;
        left: 61px;
        border: 4px solid #9FA29D;
        border-right: 4px solid transparent;
        border-left: 4px solid transparent;
        border-bottom: 4px solid transparent;
    }

    .dd.open {
        height: 80px;
        transition-property: height;
        transition-duration: .35s;
        transition-timing-function: ease;
    }

    .dd:not(.open) {
        height: 0;
        transition-property: height;
        transition-duration: .35s;
        transition-timing-function: ease;
    }

    .blocker .tab {
        height: 25px;
        width: 350%;
    }

    .blocker *:hover {
        cursor: default;
    }

    .high {
        position: relative;
        z-index: 3;
    }

</style>
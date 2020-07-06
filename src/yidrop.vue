<template>
    <nav class="yidrop-nav">
        <div v-for="(menu, menuIndex) in menuItems" :key="menuIndex" class="yidrop-dropdown">
            <button
                class="yidrop-dropbtn"
                @click="toggleVisibility(menuIndex)"
                :class="[visible[menuIndex] ? 'yidrop-up' : 'yidrop-down']"
            >
                <i>{{ menu.name }}</i>
            </button>
            <ul
                :class="[
                    visible[menuIndex] ? 'yidrop-visible-menu' : 'yidrop-hidden-menu',
                    'yidrop-dropdown-content',
                ]"
            >
                <li v-for="(link, linkIndex) in menu.links" :key="linkIndex">
                    <button :disabled="!link.active" @click="sendEmit(menuIndex, link.signal)">
                        {{ link.name }}
                    </button>
                </li>
            </ul>
        </div>
    </nav>
</template>


<style scoped>
.yidrop-up {
    background-image: url('arrow-up.svg');
}

.yidrop-down {
    background-image: url('arrow-downsvg');
}

.yidrop-visible-menu {
    display: block;
}

.yidrop-hidden-menu {
    display: none;
}

.yidrop-nav a {
    float: left;
    display: block;
    color: #f2f2f2;
    text-align: center;
    padding: 14px 16px;
    font-size: 17px;
}

.yidrop-active {
    color: white;
}

.yidrop-topnav .icon {
    display: none;
}

.yidrop-dropdown {
    float: left;
    overflow: hidden;
}

.yidrop-dropdown .yidrop-dropbtn {
    font: 400 15.3333px Arial;
    padding-right: 40px;
    background-size: 30px;
    border-right-width: 10px;
    background-repeat: no-repeat;
    cursor: pointer;
    min-height: 50px;
    outline: 0;
    border: 1px none rgba(86, 88, 88, 0.28);
    border-right-style: solid;
    color: #555656;
    background-position: right 5px center;
    text-align: center;
}

.yidrop-dropdown button {
    font-weight: normal;
}

.yidrop-dropdown-content {
    position: absolute;
    background-color: #f9f9f9;
    box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
    width: inherit;
    z-index: 2;
    float: left;
    padding: 5px 0;
    margin: 0px 0px 0px -1px;
    list-style: none;
    font-size: 14px;
    text-align: left;
    border: 1px solid rgba(0, 0, 0, 0.15);
    border-radius: 0px 0px 4px 4px;
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
    background-clip: padding-box;
    border-top: 0px;
}

.yidrop-dropdown-content button {
    width: 100%;
    background-color: #f9f9f9;
    float: none;
    padding: 12px 16px;
    text-decoration: none;
    display: block;
    text-align: left;
    min-width: 150px;
    border: none;
}

.yidrop-dropdown-content button:hover:enabled {
    background-color: #2baafe;
}

.yidrop-dropdown-content button:disabled {
    color: grey;
    cursor: default;
    pointer-events: none;
    color: #c9c9c9;
    background-color: rgb(240, 240, 240)
}

</style>

<script lang="ts">
import { Vue, Component, Prop, Watch } from 'vue-property-decorator';

interface ISubMenuItem {
    name: string;
    signal: string;
    active?: boolean;
}

export interface IMenuItem {
    name: string;
    links: Array<ISubMenuItem>;
}

@Component
export default class extends Vue {
    @Prop({
        required: true,
    })
    menuItems!: Array<IMenuItem>;
    visible = Array<Boolean>();

    @Watch('menuItems')
    menuChange() {
        this.visible.splice(0);
        for (const item of this.menuItems) {
            this.visible.push(false);
        }
    }

    sendEmit(index: number, signal: string) {
        this.visible.splice(index, 1, false);
        this.$emit(signal);
    }

    toggleVisibility(index: number) {
        const currentVisibilty = this.visible[index];
        this.visible.splice(index, 1, !currentVisibilty);
        if (!currentVisibilty) {
            this.visible.forEach((v, i) => {
                if (i != index) {
                    this.visible.splice(i, 1, false);
                }
            });
        }
    }

    hideMenus() {
        this.visible.splice(0);
        for (const item of this.menuItems) {
            this.visible.push(false);
        }
    }
}
</script>
<template>
    <header class="page-header">
        <div class="header-content">
            <div class="logo">
                <a href="/" class="brand">
                    <img alt="Vue logo"
                         class="logo"
                         src="/src/assets/images/logo.png"/>
                    Clean Vue
                </a>
            </div>

            <div class="main-navigation">

                <!-- DESKTOP MENU -->
                <nav ref="mainMenu" id="main-menu">
                    <router-link class="nav-link" :to="{ name: 'Home' }">
                        <font-awesome-icon :icon="['fas', 'home']"/>
                        Home
                    </router-link>

                    <router-link class="nav-link" :to="{ name: 'Hello' }">
                        <font-awesome-icon :icon="['fas', 'user']"/>
                        Hello
                    </router-link>

                    <router-link class="nav-link" :to="{ name: 'Dashboard' }">
                        <font-awesome-icon :icon="['fas', 'dashboard']"/>
                        Dashboard
                    </router-link>

                    <Logout v-if="authStore.loggedIn === true" @onLogout="onLogout" />

                    <router-link v-else class="nav-link" :to="{ name: 'Login' }">
                        <font-awesome-icon :icon="['fas', 'user']"/>
                        Login
                    </router-link>
                </nav>

                <!-- DARK/LIGHT MODE SWITCHER -->
                <button v-if="isDarkModeOn() === true"
                        class="pointer darkmode-toggle"
                        rel="button"
                        @click="toggleDarkMode"
                        :title="isDarkModeOn() ? 'light' : 'dark'"
                >🔆
                </button>

                <button v-else
                        class="pointer darkmode-toggle"
                        rel="button"
                        @click="toggleDarkMode"
                        :title="isDarkModeOn() ? 'light' : 'dark'"
                >🌒
                </button>

                <!-- HAMBURGER MENU -->
                <div>
                    <button @click="toggleOffcanvasMenu"
                            id="hamburger-menu-button"
                            class="primary alt margin-top-0 margin-left-0-5"
                            data-collapse-toggle="navbar-default"
                            type="button"
                            aria-controls="navbar-default"
                            aria-expanded="false"
                    >
                        <span class="sr-only">Open main menu</span>
                        <font-awesome-icon :icon="sidenav ? 'fa-solid fa-times' : 'fa-solid fa-bars'"/>
                    </button>

                    <div class="sidenav relative"
                         tabindex="-1"
                         ref="offcanvas"
                         id="main-menu-offcanvas"
                         v-click-outside="closeOnOutsideClick"
                    >
                        <a href="javascript:void(0)"
                           @click="closeOffcanvasMenu"
                           class="close-btn fs-18 absolute topright padding-0-5">
                            <font-awesome-icon :icon="['fas', 'times']"/>
                        </a>

                        <div ref="mobileMenu" id="mobile-menu">

                            <!-- MOBILE MENU -->
                            <nav id="main-menu">
                                <router-link class="nav-link" :to="{ name: 'Home' }">
                                    <font-awesome-icon :icon="['fas', 'home']"/>
                                    Home
                                </router-link>

                                <router-link class="nav-link" :to="{ name: 'Hello' }">
                                    <font-awesome-icon :icon="['fas', 'user']"/>
                                    Hello
                                </router-link>

                                <router-link class="nav-link" :to="{ name: 'Dashboard' }">
                                    <font-awesome-icon :icon="['fas', 'dashboard']"/>
                                    Dashboard
                                </router-link>

                                <Logout v-if="authStore.loggedIn === true" @onLogout="onLogout"/>

                                <router-link v-else class="nav-link" :to="{ name: 'Login' }">
                                    <font-awesome-icon :icon="['fas', 'user']"/>
                                    Login
                                </router-link>
                            </nav>

                        </div>

                    </div>

                </div>

            </div>

        </div>
    </header>


</template>

<script>
import clickOutside from "./../../directives/clickOutside";
import {authStore} from "../../store/authStore";

import Logout from "../private/Logout.vue";


export default {
    name: "Header",
    components: {
        Logout
    },
    directives: {
        clickOutside,
    },

    data() {
        return {
            authStore,
            sidenav: false,
            clickedOutside: false,
            darkMode: localStorage.getItem('darkMode') === 'true',
        };
    },

    watch: {
        // update localstorage if dark mode changes
        darkMode: function ($val) {
            console.log('Setting dark/light mode')
            localStorage.setItem('darkMode', $val);
        },
    },



    mounted() {
        this.sidenav = false;
        this.clickedOutside = false;
    },


    methods: {

        /* Offcanvas menu toggle */
        toggleOffcanvasMenu() {
            console.log('toggled menu...')
            if (this.sidenav === true) {
                this.closeOffcanvasMenu();
            } else {
                this.openOffcanvasMenu();
            }
        },


        openOffcanvasMenu() {
            this.$refs.offcanvas.style.width = "300px";
            this.sidenav = true;
            this.clickedOutside = false;
        },

        closeOffcanvasMenu() {
            this.$refs.offcanvas.style.width = "0";
            this.sidenav = false;
            this.clickedOutside = false;
        },

        closeOnOutsideClick() {
            // do not close on initial outside (of the sidebar div) click on the hamburger btn
            if (this.sidenav === true && this.clickedOutside === true) {
                this.closeOffcanvasMenu();
            } else {
                this.clickedOutside = true;
            }

        },

        /* Dark mode toggle */
        toggleDarkMode() {
            this.darkMode = !this.darkMode;
            this.$emit('darkmodechanged', this.darkMode);
        },
        /* Check if dark mode is on */
        isDarkModeOn() {
            return this.darkMode === true;
        },

        onLogout() {
            this.closeOffcanvasMenu();
        },

    },
}
</script>

<style scoped>

</style>

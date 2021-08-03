<template>
    <div>
        <header :class="(type === 'article' ? 'guide-header' : 'page-header') + (invert ? ' invert': '')" role="navigation" v-if="(hidden !== true)">
            <div class="header-content">
                <router-link :to="'/'" v-if="type === 'article' || type === 'article-wide'">
                    <img :src="logo.icon.color" alt="Bellese Logo With Gradient" class="logo color" />
                    <img :src="logo.icon.reversed" alt="Bellese Logo Solid White" class="logo reversed" />
                    <div class="site-name">
                        <img :src="logo.wordmark.color" alt="Bellese wordmark" class="wordmark color"/>
                        <img :src="logo.wordmark.reversed" alt="Bellese wordmark" class="wordmark reversed"/>
                        <span v-html="label"></span>
                    </div>
                </router-link>
                <template v-else>
                    <router-link to="/">
                        <img :src="logo.full.color" alt="Bellese Logo With Gradient" class="logo color" />
                        <img :src="logo.full.reversed" alt="Bellese Logo Solid White" class="logo reversed" />
                    </router-link>
                    <div class="site-name sr-only">Bellese</div>
                </template>
                <nav>
                    <ul :class="type === 'article-wide'? 'nav wide' : 'nav'">
                        <li class="router-link-active router-link-exact-active" style="display:none;">Placeholder</li>
                        <li class="router-link-active" style="display:none;">Placeholder</li>
                        <li 
                            v-for="(link, index) in links"
                            v-bind:key="index"
                        >
                            <a :href="link.route" v-html="link.name" v-if="link.route.startsWith('http')" target="_blank" rel="noreferrer"></a>
                            <router-link :to="link.route" v-html="link.name" v-else></router-link>
                        </li>
                    </ul>
                    <div class="mobile-nav">
                        <button v-on:click="toggleMobileMenu" :class="(showMenu === true)? 'button gradient icon-left white-bkg nav-active':'button gradient icon-left white-bkg'">
                            <img :src="icons.nav" alt="Navigation Icon" class="icon menu" aria-hidden="true"/>
                            <img :src="icons.times" alt="Close Icon" class="icon close" aria-hidden="true"/>
                            {{(showMenu === true)? 'Close':'Menu'}}
                        </button>
                        <ul :class="showMenu ? 'active' : ''">
                            <router-link to="/" class="mobile-home-link">
                                <li class="logo">
                                    <img :src="logo.full.color" alt="Bellese Logo With Gradient" class="logo color" />
                                    <img :src="logo.full.reversed" alt="Bellese Logo Solid White" class="logo reversed" />
                                </li>
                            </router-link>
                            <router-link to="/" class="mobile-home-link">
                                <li style="--animation-order: 1;">Home</li>
                            </router-link>
                            <router-link :to="link.route" v-for="(link, index) in links" :key="index" v-if="!link.route.startsWith('http')">
                                <li :style="'--animation-order: ' + (index + 2) + ';'" v-html="link.name"></li>
                            </router-link>
                            <a :href="link.route" v-for="(link, index) in links"
                               v-if="link.route.startsWith('http')" target="_blank" rel="noreferrer">
                                <li :style="'--animation-order: ' + (index + 2) + ';'" v-html="link.name"></li>
                            </a>
                            <div class="sr-only">
                                <a href="#" v-on:click.prevent="hideMobileNav">Close Navigation</a>
                            </div>
                        </ul>
                        <transition name="fade">
                            <div v-if="(showMenu === true)" class="mobile-nav-cover" v-on:click="hideMobileNav"></div>
                        </transition>
                    </div>
                </nav>
            </div>
        </header>
        <div class="feed-child-header"
             :class="(type === 'article-wide' || !type) ? 'wide' : 'standard'"
             v-if="returnNav">
            <nav>
                <router-link v-if="label === 'Careers'" :to="'/' + label.toLowerCase() + '/opportunities/'">
                    Return to {{ label }}
                </router-link>
                <router-link v-else-if="label" :to="'/' + label.toLowerCase() + '/'">
                    Return to {{ label }}
                </router-link>
                <router-link v-else :to="'/about/team/'">
                    Return to Our Team
                </router-link>
            </nav>
        </div>
    </div>
</template>

<script>
    export default {
        name: "navbar",
        data() {
            return {
                showMenu: false,
                links: []
            }
        },
        watch:{
            $route (to, from){
                this.showMenu = false;
            }
        },
        props: ['type', 'label', 'hidden', 'invert', 'returnNav', 'logo', 'icons', 'navLinks'],
        methods: {
            toggleMobileMenu: function () {
                this.showMenu = !this.showMenu
            },
            hideMobileNav: function () {
                if(this.showMenu === true) {
                    this.showMenu = false
                }
            }
        },
        created() {
            if(this.navLinks) {
                this.links = this.navLinks;
            } else {
                this.links = [
                    { route: '/about/', name: 'About Us' },
                    { route: '/blog/', name: 'Blog' },
                    { route: '/careers/', name: 'Careers' },
                    { route: '/newsroom/', name: 'Newsroom' },
                    { route: '/contact/', name: 'Contact' }
                ]
            }
        }
    }
</script>

<style scoped>

</style>
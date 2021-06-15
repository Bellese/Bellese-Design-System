<template>
    <footer id="footer" v-if="(hidden !== true)">
        <section>
            <div class="logo" v-if="logo">
                <a href="/">
                    <img :src="logo.url" :alt="logo.alt"/>
                </a>
                <p v-html="footerCTA.text" v-if="footerCTA"></p>
                <a :href="footerCTA.url" class="button inverse" v-if="footerCTA">{{ footerCTA.label }}</a>
            </div>
            <div class="col" v-for="(col, index) in cols" :index="index">
                <h3>{{ col.title }}</h3>
                <ul>
                    <li v-for="(item, index) in col.items" :index="index">
                        <a :href="item.to">{{ item.label }}</a>
                    </li>
                </ul>
            </div>
            <div class="utility">
                <span class="line toggle" v-if="darkModeState">
                    <label class="switch">
                        <input type="checkbox" :checked="darkModeState" v-on:click="darkModeToggle">
                        <span class="slider"></span>
                        <span class="sr-only">{{ darkModeState ? 'Turn Dark Mode Off':'Turn Dark Mode On'}}</span>
                    </label>
                    <!-- IE Doesn't like the switching of icons in the dark mode toggle -->
                    <span class="label" aria-hidden="true">
                        <img v-if="darkModeState" :src="darkmodeIcons[0]" alt='Dark mode icon'/>
                        <span class="invisible-separator"></span>
                        <img v-if="!darkModeState" :src="darkmodeIcons[1]" alt='Light mode icon'/>
                        <span>Dark Mode {{ darkModeState ? 'On':'Off'}}</span>
                    </span>
                </span>
                <span class="line">&copy; {{ new Date().getFullYear() }} Bellese Technologies.  All Rights Reserved.  Site designed &amp; built with &hearts; <a href="https://bellese.io/about/team/">by us</a>.<br/>We value your privacy. Here are our <a href="https://bellese.io/terms/">terms of service</a>, <a href="https://bellese.io/privacy/">privacy policy</a> and your <a to="https://bellese.io/privacy/controls/">privacy controls</a>.</span>
            </div>
        </section>
    </footer>
</template>

<script>
    export default {
        name: "app-footer",
        props: ['hidden', 'logo', 'footerCols', 'footerCTA', 'darkModeToggle', 'darkModeState', 'darkmodeIcons'],
        data() {
            return {
                cols: []
            }
        },
        created() {
            if(this.footerCols) {
                this.cols = this.footerCols;
            } else {
                this.cols = [
                    {
                        title: "Col 1",
                        items: [
                            {
                                label: "Item 1.1",
                                to: "https://bellese.io",
                            },
                            {
                                label: "Item 1.2",
                                to: "https://bellese.io",
                            },
                            {
                                label: "Item 1.3",
                                to: "https://bellese.io",
                            }
                        ]
                    },
                    {
                        title: "Col 2",
                        items: [
                            {
                                label: "Item 2.1",
                                to: "https://bellese.io",
                            },
                            {
                                label: "Item 2.2",
                                to: "https://bellese.io",
                            },
                            {
                                label: "Item 2.3",
                                to: "https://bellese.io",
                            }
                        ]
                    },
                    {
                        title: "Col 3",
                        items: [
                            {
                                label: "Item 3.1",
                                to: "https://bellese.io",
                            },
                            {
                                label: "Item 3.2",
                                to: "https://bellese.io",
                            },
                            {
                                label: "Item 3.3",
                                to: "https://bellese.io",
                            }
                        ]
                    }
                ]
            }
        }
    }
</script>

<style scoped>

</style>
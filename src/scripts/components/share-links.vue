<template>
    <div :class="(orientation === 'vertical') ? 'share-links vertical' : 'share-links'">
        <a :href="'https://twitter.com/intent/tweet?text=' + message + ' ' + url" class="twitter" target="_blank" rel="noopener">
            <svg aria-hidden="true" focusable="false" data-prefix="fab" data-icon="twitter" role="presentation" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" svg-inline="" tabindex="-1" class="svg-inline--fa fa-twitter fa-w-16"><path data-v-3d211ccf="" fill="currentColor" d="M459.37 151.716c.325 4.548.325 9.097.325 13.645 0 138.72-105.583 298.558-298.558 298.558-59.452 0-114.68-17.219-161.137-47.106 8.447.974 16.568 1.299 25.34 1.299 49.055 0 94.213-16.568 130.274-44.832-46.132-.975-84.792-31.188-98.112-72.772 6.498.974 12.995 1.624 19.818 1.624 9.421 0 18.843-1.3 27.614-3.573-48.081-9.747-84.143-51.98-84.143-102.985v-1.299c13.969 7.797 30.214 12.67 47.431 13.319-28.264-18.843-46.781-51.005-46.781-87.391 0-19.492 5.197-37.36 14.294-52.954 51.655 63.675 129.3 105.258 216.365 109.807-1.624-7.797-2.599-15.918-2.599-24.04 0-57.828 46.782-104.934 104.934-104.934 30.213 0 57.502 12.67 76.67 33.137 23.715-4.548 46.456-13.32 66.599-25.34-7.798 24.366-24.366 44.833-46.132 57.827 21.117-2.273 41.584-8.122 60.426-16.243-14.292 20.791-32.161 39.308-52.628 54.253z"></path></svg>
            <span class="sr-only">Share link to Twitter</span>
        </a>
        <a :href="'https://www.linkedin.com/sharing/share-offsite/?url=' + url + '&title=' + title + '&summary=' + message + '&source=Bellese'" class="linkedin" target="_blank" rel="noopener">
            <svg aria-hidden="true" focusable="false" data-prefix="fab" data-icon="linkedin" class="svg-inline--fa fa-linkedin fa-w-14" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><path fill="currentColor" d="M416 32H31.9C14.3 32 0 46.5 0 64.3v383.4C0 465.5 14.3 480 31.9 480H416c17.6 0 32-14.5 32-32.3V64.3c0-17.8-14.4-32.3-32-32.3zM135.4 416H69V202.2h66.5V416zm-33.2-243c-21.3 0-38.5-17.3-38.5-38.5S80.9 96 102.2 96c21.2 0 38.5 17.3 38.5 38.5 0 21.3-17.2 38.5-38.5 38.5zm282.1 243h-66.4V312c0-24.8-.5-56.7-34.5-56.7-34.6 0-39.9 27-39.9 54.9V416h-66.4V202.2h63.7v29.2h.9c8.9-16.8 30.6-34.5 62.9-34.5 67.2 0 79.7 44.3 79.7 101.9V416z"></path></svg>
            <span class="sr-only">Share link to LinkedIn</span>
        </a>
        <a :href="'https://www.facebook.com/sharer/sharer.php?display=page&u=' + url" class="facebook" target="_blank" rel="noopener">
            <svg aria-hidden="true" focusable="false" data-prefix="fab" data-icon="facebook" role="presentation" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" svg-inline="" tabindex="-1" class="svg-inline--fa fa-facebook fa-w-16"><path data-v-3d211ccf="" fill="currentColor" d="M504 256C504 119 393 8 256 8S8 119 8 256c0 123.78 90.69 226.38 209.25 245V327.69h-63V256h63v-54.64c0-62.15 37-96.48 93.67-96.48 27.14 0 55.52 4.84 55.52 4.84v61h-31.28c-30.8 0-40.41 19.12-40.41 38.73V256h68.78l-11 71.69h-57.78V501C413.31 482.38 504 379.78 504 256z"></path></svg>
            <span class="sr-only">Share link to Facebook</span>
        </a>
        <a :href="'https://pinterest.com/pin/create/button/?url=' + url + ((image !== null)? '&media=' + image : '') + '&description=' + message" class="pinterest" target="_blank" rel="noopener">
            <svg aria-hidden="true" focusable="false" data-prefix="fab" data-icon="pinterest" class="svg-inline--fa fa-pinterest fa-w-16" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 496 512"><path fill="currentColor" d="M496 256c0 137-111 248-248 248-25.6 0-50.2-3.9-73.4-11.1 10.1-16.5 25.2-43.5 30.8-65 3-11.6 15.4-59 15.4-59 8.1 15.4 31.7 28.5 56.8 28.5 74.8 0 128.7-68.8 128.7-154.3 0-81.9-66.9-143.2-152.9-143.2-107 0-163.9 71.8-163.9 150.1 0 36.4 19.4 81.7 50.3 96.1 4.7 2.2 7.2 1.2 8.3-3.3.8-3.4 5-20.3 6.9-28.1.6-2.5.3-4.7-1.7-7.1-10.1-12.5-18.3-35.3-18.3-56.6 0-54.7 41.4-107.6 112-107.6 60.9 0 103.6 41.5 103.6 100.9 0 67.1-33.9 113.6-78 113.6-24.3 0-42.6-20.1-36.7-44.8 7-29.5 20.5-61.3 20.5-82.6 0-19-10.2-34.9-31.4-34.9-24.9 0-44.9 25.7-44.9 60.2 0 22 7.4 36.8 7.4 36.8s-24.5 103.8-29 123.2c-5 21.4-3 51.6-.9 71.2C65.4 450.9 0 361.1 0 256 0 119 111 8 248 8s248 111 248 248z"></path></svg>
            <span class="sr-only">Share link to Pinterest</span>
        </a>
        <a :href="'mailto:?subject=' + title + '&body=' + message + ' ' + url" class="email" target="_blank" rel="noopener">
            <svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="envelope-square" class="svg-inline--fa fa-envelope-square fa-w-14" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><path fill="currentColor" d="M400 32H48C21.49 32 0 53.49 0 80v352c0 26.51 21.49 48 48 48h352c26.51 0 48-21.49 48-48V80c0-26.51-21.49-48-48-48zM178.117 262.104C87.429 196.287 88.353 196.121 64 177.167V152c0-13.255 10.745-24 24-24h272c13.255 0 24 10.745 24 24v25.167c-24.371 18.969-23.434 19.124-114.117 84.938-10.5 7.655-31.392 26.12-45.883 25.894-14.503.218-35.367-18.227-45.883-25.895zM384 217.775V360c0 13.255-10.745 24-24 24H88c-13.255 0-24-10.745-24-24V217.775c13.958 10.794 33.329 25.236 95.303 70.214 14.162 10.341 37.975 32.145 64.694 32.01 26.887.134 51.037-22.041 64.72-32.025 61.958-44.965 81.325-59.406 95.283-70.199z"></path></svg>
            <span class="sr-only">Share link via email</span>
        </a>
        <a v-on:click.prevent="saveLink" :class="(linkIsSaved)? 'bookmark saved' : 'bookmark'" target="_blank" rel="noopener">
            <svg v-if="linkIsSaved" aria-hidden="true" focusable="false" data-prefix="fas" data-icon="bookmark" class="svg-inline--fa fa-bookmark fa-w-12" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 384 512"><path fill="currentColor" d="M0 512V48C0 21.49 21.49 0 48 0h288c26.51 0 48 21.49 48 48v464L192 400 0 512z"></path></svg>
            <svg v-else aria-hidden="true" focusable="false" data-prefix="far" data-icon="bookmark" class="svg-inline--fa fa-bookmark fa-w-12" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 384 512"><path fill="currentColor" d="M336 0H48C21.49 0 0 21.49 0 48v464l192-112 192 112V48c0-26.51-21.49-48-48-48zm0 428.43l-144-84-144 84V54a6 6 0 0 1 6-6h276c3.314 0 6 2.683 6 5.996V428.43z"></path></svg>
            <span class="sr-only">Save Article</span>
        </a>
    </div>
</template>

<script>
    export default {
        name: "share-links",
        props: ['title', 'message', 'orientation', 'image', 'content'],
        data() {
            return {
                url: process.env.PUBLICURL + window.location.pathname,
                savedLinks: this.getSavedLinks(),
                savedIndex: null,
                linkIsSaved: false
            }
        },
        methods: {
            getSavedLinks: function() {
                let links = window.localStorage.getItem('savedLinks');
                if(links) {
                    return JSON.parse(links);
                } else {
                    return []
                }
            },
            isSaved: function() {
                let links = this.savedLinks;
                if(links && links.length > 0) {
                    for(let i = 0; i < links.length; i++) {
                        if(links[i].url === this.url) {
                            this.savedIndex = i;
                            this.linkIsSaved = true;
                            return true;
                        }
                    }
                }
                this.linkIsSaved = false;
                return false;
            },
            saveLink: function() {
                if(this.linkIsSaved === true) {
                    this.savedLinks.splice(this.savedIndex, 1);
                } else {
                    this.savedLinks.push({
                        title: this.title,
                        url: this.url,
                        excerpt: this.createExcerpt(this.content)
                    });
                }
                window.localStorage.setItem('savedLinks', JSON.stringify(this.savedLinks));
                this.isSaved();
            },
            renderMarkdown: function(markdown) {
                return this.$root.renderMarkdown(markdown);
            },
            createExcerpt: function(content, limit = 400) {
                let value = this.renderMarkdown(content);
                if (value.length > limit) {
                    value = value.substring(0, (limit - 3)).trim() + '...';
                }
                return this.$root.stripTags(value);
            },
        },
        mounted() {
            this.isSaved();
        }
    }
</script>

<style scoped>

</style>
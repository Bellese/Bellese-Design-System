<template>
    <nav class="h4 breadcrumb" v-if="breadcrumbCount > 0">
        <span v-for="(crumb, index) in breadcrumbParsed" v-if="crumb !== ''" :key="index">
            <span>/</span>
            <router-link :to="generateBreadcrumbLink(breadcrumbParsed, index)" style="text-transform:capitalize">{{crumb}}</router-link>
            <span v-if="index === breadcrumbCount">/</span>
        </span>
    </nav>
</template>

<script>
    export default {
        name: "Breadcrumb",
        props: ['breadcrumb'],
        data() {
            return {
                breadcrumbParsed: null,
                breadcrumbCount: 0
            }
        },
        methods: {
            generateBreadcrumbLink: function(breadcrumb, index) {
                let output = "/";
                for(let i = 0; i < (index + 1); i++) {
                    if(breadcrumb[i] !== '') {
                        output += breadcrumb[i] + '/';
                    }
                }
                return output;
            },
        },
        beforeMount() {
            this.scrollPosition = Math.round(window.scrollY);
            this.breadcrumbParsed = this.breadcrumb.split('/');
            for(const crumb in this.breadcrumbParsed) {
                if(this.breadcrumbParsed[crumb] !== '') {
                    this.breadcrumbCount++;
                }
            }
        },
    }
</script>

<style scoped>

</style>
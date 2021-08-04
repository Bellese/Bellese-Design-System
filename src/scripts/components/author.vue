<template>
    <div :class="(vertical === 'true' || vertical === true)? 'author vertical' : 'author'">
        <div>
            <img class="thumbnail" :src="$root.useCDN(author.profilePhoto.url)" :alt="author.Name + ' Bellese Profile Photo'" v-if="author.profilePhoto"/>
            <div :class="(isPressContact ? 'press-contact ' : '') + 'profile-name'">
                <h5 class="name">
                    <router-link v-if="author.showOnStaffPage" :to="'/about/team/' + author.Name.replace(/\s+/g, '-').toLowerCase() + '/'">{{ author.Name }}</router-link>
                    <a v-else-if="author.external_organization && author.url" :href="author.url" target="_blank" rel="noreferrer">{{ author.Name }}</a>
                    <span v-else>{{ author.Name }}</span>
                </h5>
                <div class="title">
                    <span>{{ author.jobTitle }}</span>
                    <div v-if="showOrg && author.external_organization">
                        <img class="author-org" :src="author.external_organization.logo.url" :alt="author.external_organization + ' logo'"/>
                    </div>
                    <div v-else-if="showOrg">
                        <img class="author-org" :src="logos.full" alt="Bellese logo"/>
                    </div>
                </div>
                <div v-if="isPressContact" class="contact-info">
                    <span v-if="author.phone" class="phone"><img svg-inline svg-sprite src="@bellese/bellese-design-system/src/images/icons/mobile-regular.svg"/> {{ author.phone | formatPhone }}</span>
                    <span class="email"><img svg-inline svg-sprite src="@bellese/bellese-design-system/src/images/icons/mail-bulk-regular.svg"/> <a href="mailto:press@bellese.io">press@bellese.io</a></span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "author",
        props: ['author', 'isPressContact', 'vertical', 'showOrg', 'logos']
    }
</script>

<style scoped>

</style>
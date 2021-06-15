<template>
    <router-link :to="'/about/team/' + member.slug + '/'" v-if="visible" :title="member.Name" :aria-label="member.name" tabindex="0">
        <div v-if="hasPhoto" class="profile photo" :style="'background-position: unset;background-size: cover;background-image: url(' + member.profilePhoto.formats.thumbnail.url + ')'">
            <div class="profile-name">
                <p class="name">{{member.Name}}</p>
                <p class="title">{{categories.join(', ') | truncateForIE(30)}}</p>
            </div>
        </div>
        <div v-if="!hasPhoto" class="profile">
            <div class="profile-name">
                <p class="name">{{member.Name}}</p>
                <p class="title">{{categories.join(', ') | truncateForIE(30)}}</p>
            </div>
        </div>
    </router-link>
</template>

<script>
    import '../../css/components/_team.sass'
    export default {
        name: "team-member-profile",
        data() {
            return {
                hasPhoto: false,
                visible: false,
                categories: []
            }
        },
        created() {
            this.selectImage();
            this.getCategories();
            this.checkVisibility();
        },
        methods: {
            selectImage: function() {
                if(this.member.profilePhoto !== null) {
                    this.hasPhoto = true
                }
            },
            getCategories: function() {
                for(let i = 0; i < this.member.staff_categories.length; i++) {
                    this.categories.push(this.member.staff_categories[i].Name);
                }
            },
            checkVisibility: function() {
                let category = this.selectedCategory;
                if(category.toLowerCase() === 'all') {
                    this.visible = true;
                } else if(this.categories.includes(category)) {
                    this.visible = true;
                } else {
                    this.visible = false;
                }
            }
        },
        props: ['member', 'selectedCategory'],
        watch: {
            selectedCategory: function(newVal, oldVal) { // watch it
                this.checkVisibility(newVal);
            }
        }
    }
</script>

<style scoped>

</style>
<template>
    <nav aria-roledescription="Pagination" role="navigation" class="pagination">
        <div class="pagination-container">

            <!-- Go to the previous page if the previous page exists -->
            <!-- Always visible, disabled if page is out of bounds -->
            <button v-on:click="previousPage"
                    :disabled="isBeginning(currentPage)"
                    class="start">
                <svg version="1.1" id="pagination-left-arrow-icon" focusable="false" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"
                     x="0px" y="0px" viewBox="0 0 256 512" style="enable-background:new 0 0 256 512;" xml:space="preserve">
                    <path style="fill:#FFFFFF;" d="M231.3,473.9l19.8-19.8c4.7-4.7,4.7-12.3,0-17L70.4,256L251.1,74.9c4.7-4.7,4.7-12.3,0-17l-19.8-19.8
                        c-4.7-4.7-12.3-4.7-17,0L4.9,247.5c-4.7,4.7-4.7,12.3,0,17l209.4,209.4C219,478.6,226.6,478.6,231.3,473.9z"/>
                </svg>

                Prev</button>

            <!-- Go to the first page if not already on the first page -->
            <!-- Always visible, disabled if first page is selected -->
            <button v-on:click="goToPage(0)"
                    :disabled="isBeginning(currentPage)"
                    v-if="(isEnd(currentPage) || currentPage > 2) && maxPage > 5">1</button>

            <span v-if="(isEnd(currentPage) || currentPage > 3) && maxPage > 5">...</span>

            <!-- Go 4 pages back, IF 4 pages back exists, AND within 1 page of the end  -->
            <!-- This ensures that 5 page buttons are always shown in pagination -->
            <button
                    v-on:click="goToPage(currentPage - 4)"
                    v-if="isValidPage(currentPage - 4) && (pagesFromEnd() === 0)"
                    @click="$event.target.blur()">{{ currentPage - 3 }}</button>

            <!-- Go 3 pages back, IF 3 pages back exists, AND within 2 pages of the end  -->
            <!-- This ensures that 5 page buttons are always shown in pagination -->
            <button v-on:click="goToPage(currentPage - 3)"
                    v-if="isValidPage(currentPage - 3) && (pagesFromEnd() <= 1)"
                    @click="$event.target.blur()">{{ currentPage - 2 }}</button>

            <!-- Go 2 pages back, IF 2 pages back exists  -->
            <!-- This will always show if the selected page index is at least 2 -->
            <button v-on:click="goToPage(currentPage - 2)"
                    v-if="isValidPage(currentPage - 2)"
                    @click="$event.target.blur()">{{ currentPage - 1 }}</button>

            <!-- Go 1 page back, IF 1 page back exists  -->
            <!-- This will always show if the selected page index is at least 1 -->
            <button v-on:click="previousPage"
                    v-if="isValidPage(currentPage - 1) && (currentPage > 0)"
                    @click="$event.target.blur()">{{ currentPage }}</button>

            <!-- The selected page  -->
            <!-- Always disabled.  Always visible. -->
            <button v-on:click="goToPage(currentPage)" disabled class="current">{{ currentPage + 1 }}</button>

            <!-- Go 1 page forward, IF 1 page forward exists  -->
            <!-- This will always show if the selected page index is less than the maximum page index -->
            <button v-on:click="nextPage"
                    v-if="isValidPage(currentPage + 1) && (currentPage < maxPage)"
                    @click="$event.target.blur()">{{ currentPage + 2 }}</button>

            <!-- Go 2 pages forward, IF 2 pages forward exists  -->
            <!-- This will always show if the selected page index is at least 2 less than the maximum page index -->
            <button v-on:click="goToPage(currentPage + 2)"
                    v-if="isValidPage(currentPage + 2)"
                    @click="$event.target.blur()">{{ currentPage + 3 }}</button>

            <!-- Go 3 pages forward, IF 3 pages forward exists, AND within 2 pages of the beginning -->
            <!-- This ensures that 5 page buttons are always shown in pagination -->
            <button v-on:click="goToPage(currentPage + 3)"
                    v-if="isValidPage(currentPage + 3) && (pagesFromBeginning() <= 1)"
                    @click="$event.target.blur()">{{ currentPage + 4 }}</button>

            <!-- Go 4 pages forward, IF 4 pages forward exists, AND if the first page is selected -->
            <!-- This ensures that 5 page buttons are always shown in pagination -->
            <button v-on:click="goToPage(currentPage + 4)"
                    v-if="isValidPage(currentPage + 4) && (pagesFromBeginning() === 0)"
                    @click="$event.target.blur()">{{ currentPage + 5 }}</button>

            <span v-if="!isEnd(currentPage) && currentPage < maxPage - 3">...</span>

            <!-- Go to the last page if not already on the last page -->
            <!-- Always visible, disabled if last page is selected -->
            <button v-on:click="goToPage(maxPage)"
                    :disabled="isEnd(currentPage)"
                    v-if="!isEnd(currentPage) && currentPage < maxPage - 2 && maxPage > 5">{{ maxPage + 1 }}</button>

            <!-- Go to the next page if the next page exists -->
            <!-- Always visible, disabled if page is out of bounds -->
            <button v-on:click="nextPage"
                    :disabled="isEnd(currentPage)"
                    class="end">
                <svg version="1.1" id="pagination-right-arrow-icon" focusable="false" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"
                     x="0px" y="0px" viewBox="0 0 256 512" style="enable-background:new 0 0 256 512;" xml:space="preserve">
                    <path style="fill:#FFFFFF;" d="M24.7,38.1L4.9,57.9c-4.7,4.7-4.7,12.3,0,17L185.6,256L4.9,437.1c-4.7,4.7-4.7,12.3,0,17l19.8,19.8
                        c4.7,4.7,12.3,4.7,17,0l209.4-209.4c4.7-4.7,4.7-12.3,0-17L41.7,38.1C37,33.4,29.4,33.4,24.7,38.1z"/>
                </svg>
                Next</button>
        </div>
    </nav>
</template>

<script>
    export default {
        name: "pagination",
        data() {
            return {
                allContent: this.content,
                maxPage: 0,
                currentPage: this.getPage,
                selectedContent: []
            }
        },
        methods: {
            insideBounds(index) {
                return ((index > 0) && (index < this.maxPage));
            },
            isBeginning(index) {
                return (index === 0);
            },
            isEnd(index) {
                return (index === this.maxPage);
            },
            isValidPage(index) {
                return this.insideBounds(index) || this.isBeginning(index) || this.isEnd(index);
            },
            pagesFromEnd() {
                return this.maxPage-this.currentPage;
            },
            pagesFromBeginning() {
                return this.currentPage;
            },
            getPage() {
                if(this.$route.query.page) {
                    this.currentPage = this.$route.query.page - 1;
                } else {
                    this.currentPage = 0
                }
                return this.currentPage;
            },
            setPage(page) {
                this.currentPage = page;
                this.$router.push({ path: this.$route.path, query: { page: page + 1 }}).catch(() => {})
            },
            calcMaxPage() {
                return Math.ceil((this.allContent.length / this.perPage)) - 1;
            },
            selectContent() {
                let temp = [];
                let offset = this.getPage() * this.perPage;
                for(let i = 0; i < this.perPage; i++) {
                    if(this.allContent[offset + i]) {
                        temp[i] = this.allContent[offset + i];
                    }
                }
                this.selectedContent = temp;
                this.$emit('update', this.$data);
            },
            nextPage() {
                this.setPage(this.currentPage + 1);
                this.selectContent();
            },
            previousPage() {
                this.setPage(this.currentPage - 1);
                this.selectContent();
            },
            goToPage(page) {
                this.setPage(page);
                this.selectContent();
            }
        },
        created() {
            this.maxPage = this.calcMaxPage();
            this.selectContent();
            // If page queried is out of bounds, reset to first page
            if((this.currentPage > this.maxPage) || (this.currentPage < 0)) {
                this.setPage(0);
                this.selectContent();
            }
        },
        watch: {
            $route (to, from) {
                this.goToPage(this.getPage());
            }
        },
        props: ['content', 'perPage']
    }
</script>

<style scoped>

</style>
<template>
  <div>
    <section class="hero page slim" v-if="category || breadcrumb">
      <div class="hero-content">
        <div>
          <h1 class="micro">{{ category }}</h1>
          <h2 class="accent white-accent">{{ title }}</h2>
        </div>
      </div>
    </section>
    <div class="page-container">
      <div class="page-content">
        <div class="content-header" v-if="!category && !breadcrumb">
          <h1 class="accent" >{{ title }}</h1>
        </div>
        <Breadcrumb :breadcrumb="breadcrumb"/>
      </div>
    </div>
    <div class="page-container left-aside-page">
      <main class="left-aside-content-container" role="main">
        <section class="page-nav">
          <aside ref="page-nav">
            <h4>Sections</h4>
            <ul class="category-list">
              <li v-for="(section, index) in sectionLabels" ref="category-list-item">
                <a :href="'#' + section.id">{{ section.name | capitalizeFirst }}</a>
              </li>
            </ul>
          </aside>
        </section>
        <section class="page-content" ref="page-content" id="main-content" style="margin-top:16px">
          <slot></slot>
        </section>
      </main>
    </div>
  </div>
</template>

<script>
    import Breadcrumb from "./breadcrumb.vue";
    export default {
        name: "DocPage",
      components: {
          Breadcrumb
      },
      data() {
            return {
                navPosition: 0,
                sectionsEls: null,
                sectionLabels: [],
                mainContentEl: null,
                contentTop: 0,
                contentBottom: 0,
                scrollPosition: 0
            }
        },
        props: [
            'category',
            'title',
            'breadcrumb'
        ],
        methods: {
            calcScrollValues: function() {
                this.scrollPosition = Math.round(window.scrollY);

                this.sectionsEls.forEach((el, index) => {
                    const elTop = el.getBoundingClientRect().top;
                    const elBottom = el.getBoundingClientRect().bottom;
                    if(index === 0) {
                        if(this.scrollPosition < this.contentTop) {
                            // First item active if page has not been scrolled yet
                            this.$refs['category-list-item'][0].classList.add("active");
                        } else if (elTop >= 40 || elBottom <= 40) {
                            // Not Active
                            this.$refs['category-list-item'][0].classList.remove("active");
                        } else if (elTop <= 40 && elBottom >= 40) {
                            // Active
                            this.$refs['category-list-item'][0].classList.add("active");
                        }
                    } else {
                        if (elTop >= 40 || elBottom <= 40) {
                            // Not Active
                            this.$refs['category-list-item'][index].classList.remove("active");
                        } else if (elTop <= 40 && elBottom >= 40) {
                            // Active
                            this.$refs['category-list-item'][index].classList.add("active");
                        }
                    }
                });
            }
        },
        beforeMount() {
            this.scrollPosition = Math.round(window.scrollY);
        },
        mounted() {
            this.$refs['page-nav'].style.position = "relative";
            this.$refs['page-nav'].style.top = "0";
            this.$refs['page-nav'].style.position = "";
            this.$refs['page-nav'].style.top = "";
            this.sectionsEls = document.querySelectorAll('#main-content > article');
            this.sectionsHeaders = document.querySelectorAll('#main-content > article > h3');
            this.mainContentEl = document.querySelector('#main-content');
            this.contentTop = this.mainContentEl.getBoundingClientRect().top;
            this.contentBottom = this.mainContentEl.getBoundingClientRect().bottom;
            this.sectionsHeaders.forEach((el, index) => {
                this.sectionLabels.push({
                    name: el.innerText,
                    id: this.sectionsHeaders[index].id
                });
            });
        },
        updated() {
            this.$nextTick(function () {
                this.mainContentEl = document.querySelector('#main-content');
                this.contentTop = this.mainContentEl.getBoundingClientRect().top;
                this.contentBottom = this.mainContentEl.getBoundingClientRect().bottom;
                this.navPosition = this.$refs['page-nav'].getBoundingClientRect().top;
                window.addEventListener('scroll', this.calcScrollValues);
                this.calcScrollValues();
            })
        },
        watch: {
            scrollPosition: function(newScrollPosition) {
                if ((newScrollPosition > (this.navPosition-40)) && (newScrollPosition < (this.contentBottom - this.$refs['page-nav'].offsetHeight - 80))) {
                    this.$refs['page-nav'].style.position = "fixed";
                    this.$refs['page-nav'].style.top = "40px";
                } else if(newScrollPosition >= (this.contentBottom - this.$refs['page-nav'].offsetHeight - 80)) {
                    this.$refs['page-nav'].style.position = "absolute";
                    this.$refs['page-nav'].style.top = (this.contentBottom - this.$refs['page-nav'].offsetHeight - 40) + "px";
                } else {
                    this.$refs['page-nav'].style.position = "";
                    this.$refs['page-nav'].style.top = "";
                }
            }
        },
        destroyed() {
            window.removeEventListener('scroll', this.calcScrollValues);
        },
    }
</script>

<style scoped>
    #main-content > article {
        margin-bottom: 0;
        padding-top: 24px;
        padding-bottom: 24px;
    }
</style>
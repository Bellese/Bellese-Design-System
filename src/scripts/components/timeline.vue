<template>
    <div class="timeline">
        <!-- Component does not support IE -->
        <section :style="{ visibility: (offsetSet)? 'visible' : 'hidden' }"
                 v-touch:swipe.left="swipeNext()"
                 v-touch:swipe.right="swipePrev()">
            <div class="event spacer"></div>
            <div v-for="(event, index) in events"
                 :aria-hidden="!(index === selectedIndex)"
                 :class="['event',
                    (!eventExists(index+1)) ? 'last' : '',
                    (!eventExists(index-1)) ? 'first' : '']"
                 :style="{ transform: 'translate3d(' + adjustedOffset + 'px,0,0)' }">
                <div class="event-title">
                    <h3 v-html="event.date"></h3>
                </div>
                <span class="timeline-marker"></span>
                <h4 v-html="event.title"></h4>
                <p v-html="renderMarkdown(event.content)" class="small"></p>
            </div>
            <div class="event spacer"></div>
        </section>
        <button v-on:click="prevEvent()"
                :disabled="selectedIndex === 0"
                class="timelinePrev">
            <svg version="1.1" id="pagination-left-arrow-icon" focusable="false" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"
                 x="0px" y="0px" viewBox="0 0 256 512" style="enable-background:new 0 0 256 512;" xml:space="preserve">
                <path style="fill:#FFFFFF;" d="M231.3,473.9l19.8-19.8c4.7-4.7,4.7-12.3,0-17L70.4,256L251.1,74.9c4.7-4.7,4.7-12.3,0-17l-19.8-19.8
                    c-4.7-4.7-12.3-4.7-17,0L4.9,247.5c-4.7,4.7-4.7,12.3,0,17l209.4,209.4C219,478.6,226.6,478.6,231.3,473.9z"/>
            </svg>
            <span class="sr-only">Previous</span>
        </button>
        <button v-on:click="nextEvent()"
                :disabled="selectedIndex === (totalEvents - 1)"
                class="timelineNext">
            <svg version="1.1" id="pagination-right-arrow-icon" focusable="false" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"
                 x="0px" y="0px" viewBox="0 0 256 512" style="enable-background:new 0 0 256 512;" xml:space="preserve">
                <path style="fill:#FFFFFF;" d="M24.7,38.1L4.9,57.9c-4.7,4.7-4.7,12.3,0,17L185.6,256L4.9,437.1c-4.7,4.7-4.7,12.3,0,17l19.8,19.8
                    c4.7,4.7,12.3,4.7,17,0l209.4-209.4c4.7-4.7,4.7-12.3,0-17L41.7,38.1C37,33.4,29.4,33.4,24.7,38.1z"/>
            </svg>
            <span class="sr-only">Next</span>
        </button>
    </div>
</template>

<script>
    import '@bellese/bellese-design-system/src/styles/components/_timeline.sass'
    import _ from 'lodash'

    export default {
        name: "timeline",
        data() {
            return {
                selectedIndex: this.totalIndexes,
                offset: 0,
                elementWidth: 0,
                totalEventsWidth: 0,
                adjustedOffset: 0,
                viewportWidth: 0,
                offsetSet: false
            }
        },
        props: ['events'],
        computed: {
            totalEvents: function() {
                return this.events.length;
            },
            totalIndexes: function() {
                return this.totalEvents - 1;
            },
            getNextEventIndex: function() {
                const nextEvent = this.selectedIndex + 1;
                return nextEvent <= this.totalEvents - 1 ? nextEvent : false;
            },
            getPreviousEventIndex: function() {
                const prevEvent = this.selectedIndex - 1;
                return prevEvent >= 0 ? prevEvent : false;
            },
            eventElements: function() {
                return this.$el.querySelectorAll('div.event');
            }
        },
        methods: {
            eventExists(index) {
                return (index >= 0 && index <= this.totalEvents - 1);
            },
            goToEvent: function(index, totalWidth = this.totalEventsWidth, elementWidth = this.elementWidth) {
                if(this.eventExists(index)) {
                    this.selectedIndex = index;
                    let offset = this.calculateOffset(this.totalIndexes - this.selectedIndex);
                    this.moveElements(offset);
                }
            },
            nextEvent: function() {
                this.goToEvent(this.getNextEventIndex);
            },
            prevEvent: function() {
                this.goToEvent(this.getPreviousEventIndex);
            },
            setTotalIndexes: function() {
                this.selectedIndex = this.totalEvents - 1;
            },
            calculateOffset: function(indexOffset) {
                return indexOffset * this.elementWidth;
            },
            updateViewportOffsets: _.debounce(function() {
                this.elementWidth = this.eventElements[0].offsetWidth + (this.$el.offsetWidth * 0.02);
                this.totalEventsWidth = this.elementWidth * this.totalEvents;
                this.viewportWidth = window.innerWidth;
                this.goToEvent(this.selectedIndex);
            }, 500),
            moveElements: function(offset) {
                this.adjustedOffset = (-(this.totalEventsWidth/2)) + offset + (this.elementWidth/2);
                this.offsetSet = true;
            },
            initializeTimeline: function(event = null) {
                let currentWidth = window.innerWidth;
                if(event && currentWidth !== this.viewportWidth) {
                    this.offset = 0;
                    this.elementWidth = 0;
                    this.totalEventsWidth = 0;
                    this.adjustedOffset = 0;
                    this.offsetSet = false;
                    this.$nextTick(() => {
                        this.updateViewportOffsets(true);
                    });
                } else if(currentWidth !== this.viewportWidth) {
                    this.$nextTick(() => {
                        this.updateViewportOffsets();
                    });
                }
            },
            enableAnimation: function() {
                let elements = this.eventElements;
                setTimeout(function(){
                    for(let i = 0; i < elements.length; i++) {
                        elements[i].style.transition = 'all 500ms ease';
                    }
                }, 500);
            },
            swipePrev() {
                return () => {
                    this.prevEvent();
                }
            },
            swipeNext() {
                return () => {
                    this.nextEvent();
                }
            },
            renderMarkdown(input) {
                return this.$root.renderMarkdown(input);
            }
        },
        watch: {
            elementWidth: function () {
                this.goToEvent(this.selectedIndex);
            },
            totalEventsWidth: function () {
                this.goToEvent(this.selectedIndex);
            },
            offsetSet: function(newval, oldval) {
                if(oldval === false && newval === true) {
                    this.enableAnimation();
                }
            }
        },
        created() {
            window.addEventListener("resize", this.initializeTimeline);
            window.addEventListener("fullscreenChange", this.initializeTimeline);
        },
        destroyed() {
            window.removeEventListener("resize", this.initializeTimeline);
            window.addEventListener("fullscreenChange", this.initializeTimeline);
        },
        mounted() {
            this.setTotalIndexes();
            this.initializeTimeline();
        }

    }
</script>

<style scoped>

</style>
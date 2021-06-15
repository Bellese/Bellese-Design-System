<template>
    <div id='remote-map' class="bellese-remote-map">
        <img svg-inline src="../../images/bellese_remote_map.svg" alt="Bellese Remote Work Map"/>
    </div>
</template>

<script>
    const stateData = require('../states.json');
    import '../../css/components/_remote-map.sass';
    export default {
        name: "remote-map",
        data() {
            return {
                rootData: this.$root.$data,
                localDark: false,
                states: stateData
            };
        },
        methods: {
            checkDarkMode: function() {
                let self = this;
                if(this.rootData.darkMode === true) {
                    if(this.localDark !== true) {
                        // switch to dark mode
                        this.localDark = true;
                    }
                } else {
                    if(this.localDark === true) {
                        // switch to light mode
                        this.localDark = false;
                    }
                }
                setTimeout(function() {
                    self.checkDarkMode();
                }, 1000);
            },
            convertValueToColor: function(int) {
                switch(int) {
                    case 0:
                        return "rgb(255, 255, 255)";
                    case 1:
                        return "rgb(243, 231, 249)";
                    case 2:
                        return "rgb(231, 206, 244)";
                    case 3:
                        return "rgb(220, 182, 239)";
                    case 4:
                        return "rgb(208, 158, 234)";
                    case 5:
                        return "rgb(196, 133, 229)";
                    case 6:
                        return "rgb(186, 109, 224)";
                    case 7:
                        return "rgb(176, 86, 220)";
                    case 8:
                        return "rgb(165, 64, 215)";
                    case 9:
                        return "rgb(155, 44, 211)";
                    default:
                        return "rgb(146, 30, 207)";
                }
            }
        },
        mounted() {
            for(const state in this.states) {
                if(this.states[state].marker === false) {
                    document.getElementById("marker_" + state).style.display = "none";
                }
                document.getElementById("map_" + state).style.fill =
                    this.convertValueToColor(this.states[state].count);
            }
        }
    };
</script>
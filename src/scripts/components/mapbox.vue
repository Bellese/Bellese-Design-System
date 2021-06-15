<template>
    <div id='mapbox' class="bellese-mapbox"></div>
</template>

<script>
    import '../../css/components/_mapbox.sass'
    import Mapbox from "mapbox-gl";

    export default {
        name: "mapbox",
        data() {
            return {
                rootData: this.$root.$data,
                localDark: false,
                mapInstance: null,
                marker: null,
                popup: null,
                accessToken: process.env.MAPBOX_PUBLIC_ACCESS_TOKEN,
                mapStyle: 'mapbox://styles/mapbox/streets-v11',
                mapCenter: (this.center) ? this.center : [-76.7868697,39.4132911],
                mapMarkerCoords: [-76.8021736,39.412697],
                mapZoom: (this.zoom) ? this.zoom : 13,
            };
        },
        props: ['center', 'zoom', 'nomarker'],
        methods: {
            checkDarkMode: function() {
                let self = this;
                if(this.rootData.darkMode === true) {
                    if(this.localDark !== true) {
                        this.mapStyle = 'mapbox://styles/mapbox/dark-v10';
                        this.mapInstance.setStyle(this.mapStyle);
                        this.marker.remove();
                        this.createMarker('#c45de9');
                        this.localDark = true;
                    }
                } else {
                    if(this.localDark === true) {
                        this.mapStyle = 'mapbox://styles/mapbox/streets-v11';
                        this.mapInstance.setStyle(this.mapStyle);
                        this.marker.remove();
                        this.createMarker('#63167E');
                        this.localDark = false;
                    }
                }
                setTimeout(function() {
                    self.checkDarkMode();
                }, 1000);
            },
            createMarker: function(colorHEX) {
                this.marker = new Mapbox.Marker({color: colorHEX})
                    .setLngLat(this.mapMarkerCoords)
                    .addTo(this.mapInstance);
            }
        },
        mounted() {
            Mapbox.accessToken = this.accessToken;
            this.mapInstance = new Mapbox.Map({
                container: 'mapbox',
                style: this.mapStyle, // stylesheet location
                center: this.mapCenter, // starting position [lng, lat]
                zoom: this.mapZoom, // starting zoom
                attributionControl: false
            });
            this.mapInstance.addControl(new Mapbox.NavigationControl());
            if(this.nomarker !== "true") {
                this.popup = new Mapbox.Popup({ offset: {'left': [25, -15]}, anchor: 'left', closeButton: false, closeOnClick: false })
                    .setLngLat(this.mapMarkerCoords)
                    .setHTML('<img src="/assets/Horizontal_RGB_Gradient.svg" class="logo-light" alt="Bellese Logo"/>' +
                        '<img src="/assets/Horizontal_web_Reversed.svg" class="logo-dark" alt="Bellese Logo"/>' +
                        '<p>300 Red Brook Blvd, Suite 100<br/>Owings Mills, MD 21117</p>' +
                        '<a href="https://g.page/Bellese" target="_blank">Get Directions</a>')
                    .addTo(this.mapInstance);
            }
            this.createMarker('#63167E');
            this.mapInstance.resize();
            this.checkDarkMode();
        }
    };
</script>
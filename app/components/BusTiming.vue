<template>
    <Page>
        <ActionBar :title="app.title + ' ' + app.version" class="action-bar" />

        <StackLayout>
            <!--Add your page content here-->

            <ListView class="list-group" for="bus in busdata.services" style="height:1250px" v-if="app.mode == 'view_bus' && loaded.busdata">
                <v-template>

                    <StackLayout flexDirection="row" class="list-group-item">
                        <Label :text="bus.no" class="list-group-item-heading own-header" />
                        <!-- DISPLAY TRUE TIME IF BUS ARRIVAL TIME IS ABOVE 1 MIN -->
                        <Label v-if="Math.floor(bus.next.duration_ms / 60000) > 1" class="list-group-item-heading">{{Math.floor(bus.next.duration_ms
                            / 60000) + ' Mins' }}</Label>
                        <!-- SHOWS ARRIVING IF BUS ARRIVAL TIME IS UNDER 1 MIN -->
                        <Label v-else text="Arriving" class="list-group-item-heading own-header" />

                    </StackLayout>

                </v-template>
            </ListView>
            <!-- SHOWS LOADING WHILE LOADING BUS STOP DATA -->
            <Label v-else>Loading.. </Label>

        </StackLayout>

    </Page>
</template>

<script>
    var appconfig = require("../package.json")

    export default {

        props: ['busstopnumber'],
        
        data() {
            return {

                app: {
                    title: "",
                    version: "",
                    mode: "?"
                },

                busdata: [],

                loaded: {
                    busdata: false,
                },

            }
        },

        methods: {

            getBusStopTiming(number) {

                console.log(number)
                
                this.app.mode = "view_bus"
                this.loaded.busdata = false

                fetch("https://arrivelah.herokuapp.com/?id=" + number)

                    .then(response => response.json())

                    .then(json => {
                        console.log(json)
                        this.busdata = json
                        this.loaded.busdata = true

                    })
            },

        },

        mounted() {
            this.app.title = appconfig.name
            this.app.version = appconfig.version
            this.getBusStopTiming(this.busstopnumber)
        }

    }
</script>

<style scoped>
    /* PCOLOR: #e24747 */

    .list-group-item-heading {
        color: #161616;
    }

    StackLayout {
        margin: 0 10;

    }

    ActionBar {
        background-color: #e24747;
        color: #ffffff;
    }

    Button {
        margin: 5 0;
        color: white;
        background-color: #e24747;
        border-radius: 1000;
        height: 38;
    }

    .own-header {
        color: #e24747;
        font-size: 18;
        font-weight: 700;
    }

    TextField {
        margin: 5 0;
    }
</style>
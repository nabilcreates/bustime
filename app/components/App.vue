<template>
    <Page class="page">
        <ActionBar :title="app.title + ' ' + app.version" class="action-bar" />

        <StackLayout class="home-panel">
            <!--Add your page content here-->

            <TextField v-model="input" @textChange="verifyInput()" />

            <Button @tap="getBusStopData()"> Back </Button>

            <ListView class="list-group" for="busstop in busstopdata" @itemTap="busstoptap" style="height:1250px" v-if="app.mode == 'search' && loaded.busstopdata">
                <v-template>
                    <StackLayout flexDirection="row" class="list-group-item">

                        <!-- BUS STOP NAME -->
                        <Label :text="busstop.name" class="list-group-item-heading own-header" />

                        <!-- BUS STOP NUMBER -->
                        <Label :text="busstop.no" class="list-group-item-heading" />

                    </StackLayout>
                </v-template>
            </ListView>

            <ListView class="list-group" for="bus in busdata.services" style="height:1250px" v-if="app.mode == 'view_bus' && loaded.busdata">
                <v-template>

                    <StackLayout flexDirection="row" class="list-group-item">
                        <Label :text="bus.no" class="list-group-item-heading own-header" />
                        <!-- DISPLAY TRUE TIME IF BUS ARRIVAL TIME IS ABOVE 1 MIN -->
                        <Label v-if="Math.floor(bus.next.duration_ms / 60000) > 1" class="list-group-item-heading">{{Math.floor(bus.next.duration_ms
                            / 60000) + ' Mins' }}</Label>
                        <!-- SHOWS ARRIVING IF BUS ARRIVAL TIME IS UNDER 1 MIN -->
                        <Label v-else text="Arriving" class="list-group-item-heading" />

                    </StackLayout>

                </v-template>
            </ListView>
            <!-- SHOWS LOADING WHILE LOADING BUS STOP DATA -->
            <Label v-else>Loading... </Label>

        </StackLayout>

    </Page>
</template>

<script>
    var appconfig = require("../package.json")

    export default {
        data() {
            return {

                app: {
                    title: "",
                    version: "",
                    mode: "?"
                },

                busstopdata: [],
                busdata: [],

                loaded: {
                    busstopdata: false,
                    busdata: false,
                },

                input: "Enter a bus stop code"

            }
        },

        methods: {

            verifyInput() {
                if (this.input == "") {
                    this.getBusStopData()
                } else {
                    this.getBusStopTiming(this.input)
                }
            },

            getBusStopData() {

                this.app.mode = "search"
                this.loaded.busstopdata = false;

                fetch("https://busrouter.sg/data/2/bus-stops.json")
                    .then(response => response.json())
                    .then(json => {
                        this.busstopdata = json
                        this.loaded.busstopdata = true;
                    })
            },

            busstoptap(args) {
                var busstopnum = this.busstopdata[args.index].no
                console.log(busstopnum)
                this.getBusStopTiming(busstopnum)
            },

            getBusStopTiming(number) {

                this.app.mode = "view_bus"
                this.loaded.busdata = false

                fetch("https://arrivelah.herokuapp.com/?id=" + number)
                    .then(response => response.json())
                    .then(json => {
                        console.log(json)
                        this.busdata = json
                        this.loaded.busdata = true

                    })
            }

        },

        mounted() {
            this.app.title = appconfig.name
            this.app.version = appconfig.version
            this.getBusStopData()
        }

    }
</script>

<style scoped>
    Page {
        background: #161616;

    }

    StackLayout {
        color: white;
        margin: 0 5;
    }

    ActionBar {
        background-color: #53ba82;
        color: #ffffff;
    }

    Button {
        background-color: #53ba82;
        border-radius: 1000;
        color: #161616;
    }

    .own-header {
        color: #53ba82;
        font-weight: 700;
    }
</style>
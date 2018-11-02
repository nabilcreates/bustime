<template>
    <Page>
        <ActionBar :title="busstopname + '(' +busstopnumber + ')'" class="action-bar" />
        <ActionBar v-if="!busstopname" :title="'(' +busstopnumber + ')'" class="action-bar" />


        <!-- <PullToRefresh @refresh="refreshList"> -->
            <StackLayout>

                <Button @tap="refreshList">Refresh</Button>
                
                <ListView class="list-group" for="bus in busdata.services" v-if="app.mode == 'view_bus' && loaded.busdata">
                    <v-template>

                        <StackLayout class="list-group-item">

                            <!-- DISPLAY BUS NUMBER -->
                            <Label :text="bus.no" class="list-group-item-heading own-header" />

                            <!-- BUS TIMING -->
                            <!-- DISPLAY TRUE TIME IF BUS ARRIVAL TIME IS ABOVE 1 MIN -->
                            <Label v-if="Math.floor(bus.next.duration_ms / 60000) > 1" class="list-group-item-heading">🕗 {{Math.floor(bus.next.duration_ms / 60000) + ' Mins' }}</Label>
                            <!-- SHOWS ARRIVING IF BUS ARRIVAL TIME IS UNDER 1 MIN -->
                            <Label v-else text="🕗 Arriving" class="list-group-item-heading" />

                            <!-- BUS FEATURE -->
                            <Label v-if="bus.next.feature == 'WAB'" textWrap="true" class="list-group-item-heading">☑️ Wheelchair </Label>
                            <Label v-else textWrap="true" class="list-group-item-heading">❎ Wheelchair </Label>

                            <!-- BUS TYPE (SINGLE DECK OR DOUBLE DECK) -->
                            <!-- SHOW SINGLE DECK IF BUS.NEXT.TYPE IS SD -->
                            <Label v-if="bus.next.type == 'SD'" textWrap="true" class="list-group-item-heading">🚌 Single Deck</Label>
                            <!-- SHOW DOUBLE DECK IF BUS.NEXT.TYPE IS DD -->
                            <Label v-if="bus.next.type == 'DD'" textWrap="true" class="list-group-item-heading">🚌 Double Deck</Label>


                        </StackLayout>

                    </v-template>
                </ListView>

                <!-- SHOWS LOADING WHILE LOADING BUS STOP DATA -->
                <Label v-else>Loading...</Label>
            </StackLayout>
        <!-- </PullToRefresh> -->



    </Page>
</template>

<script>
    var appconfig = require("../package.json")

    export default {

        props: ['busstopnumber', 'busstopname'],

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

            refreshList(args) {
                var pullRefresh = args.object;
                this.getBusStopTiming(this.busstopnumber)
                setTimeout(function () {
                    pullRefresh.refreshing = false;
                }, 1000);
            }


        },

        mounted() {

            // RUN GETBUSSTOPTIMING WITH THE BUSSTOPNUMBER PROP
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
        margin: 10 0;
        color: white;
        background-color: #e24747;
        border-radius: 1000;
        height: 38;
    }

    .own-header {
        color: #e24747;
        font-size: 23;
        font-weight: 700;
    }

    TextField {
        margin: 5 0;
    }
</style>
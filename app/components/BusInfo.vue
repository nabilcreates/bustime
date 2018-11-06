<template>
    <Page>
        <ActionBar :title="'Timing For Subsequent Buses'" class="action-bar" />


        <!-- <PullToRefresh @refresh="refreshList"> -->
        <StackLayout>


            <Button @tap="refreshList">Refresh</Button>

            
            <StackLayout v-if="loaded.busdata">

            <!-- BUTTON WITH BUS NUMBER IN OT -->
            <Button @tap="navigateToBusRoute">{{busdata.services[index].no}}</Button>

                <Label text="1st Bus" class="list-group-item-heading own-header" />

                <!-- BUS TIMING -->
                <!-- DISPLAY TRUE TIME IF BUS ARRIVAL TIME IS ABOVE 1 MIN -->
                <Label v-if="Math.floor(busdata.services[index].next.duration_ms / 60000) > -1" class="list-group-item-heading">🕗 {{Math.floor(busdata.services[index].next.duration_ms / 60000) + ' Mins' }}</Label>
                <!-- SHOWS ARRIVING IF BUS ARRIVAL TIME IS UNDER 1 MIN -->
                <Label v-else text="🕗 Arriving" class="list-group-item-heading" />

                <!-- BUS FEATURE -->
                <Label v-if="busdata.services[index].next.feature == 'WAB'" textWrap="true" class="list-group-item-heading">☑️ Wheelchair</Label>
                <Label v-else textWrap="true" class="list-group-item-heading">❎ Wheelchair </Label>

                <!-- BUS TYPE (SINGLE DECK OR DOUBLE DECK) -->
                <!-- SHOW SINGLE DECK IF busdata.services[index].NEXT.TYPE IS SD -->
                <Label v-if="busdata.services[index].next.type == 'SD'" textWrap="true" class="list-group-item-heading">🚌 Single Deck</Label>
                <!-- SHOW DOUBLE DECK IF busdata.services[index].NEXT.TYPE IS DD -->
                <Label v-if="busdata.services[index].next.type == 'DD'" textWrap="true" class="list-group-item-heading">🚌 Double Deck</Label>
            </StackLayout>

            <!-- NEXT2 -->
            <StackLayout v-if="loaded.busdata">

                <!-- DISPLAY BUS NUMBER -->
                <Label text="2nd Bus" class="list-group-item-heading own-header" />

                <!-- BUS TIMING -->
                <!-- DISPLAY TRUE TIME IF BUS ARRIVAL TIME IS ABOVE 1 MIN -->
                <Label v-if="Math.floor(busdata.services[index].next2.duration_ms / 60000) > -1" class="list-group-item-heading">🕗 {{Math.floor(busdata.services[index].next2.duration_ms / 60000) + ' Mins' }}</Label>
                <!-- SHOWS ARRIVING IF BUS ARRIVAL TIME IS UNDER 1 MIN -->
                <Label v-else text="🕗 Arriving" class="list-group-item-heading" />

                <!-- BUS FEATURE -->
                <Label v-if="busdata.services[index].next2.feature == 'WAB'" textWrap="true" class="list-group-item-heading">☑️ Wheelchair</Label>
                <Label v-else textWrap="true" class="list-group-item-heading">❎ Wheelchair </Label>

                <!-- BUS TYPE (SINGLE DECK OR DOUBLE DECK) -->
                <!-- SHOW SINGLE DECK IF busdata.services[index].NEXT2.TYPE IS SD -->
                <Label v-if="busdata.services[index].next2.type == 'SD'" textWrap="true" class="list-group-item-heading">🚌 Single Deck</Label>
                <!-- SHOW DOUBLE DECK IF busdata.services[index].NEXT2.TYPE IS DD -->
                <Label v-if="busdata.services[index].next2.type == 'DD'" textWrap="true" class="list-group-item-heading">🚌 Double Deck</Label>
            </StackLayout>

            <!-- NEXT3 -->
            <StackLayout v-if="loaded.busdata">

                <!-- DISPLAY BUS NUMBER -->
                <Label text="3rd Bus" class="list-group-item-heading own-header" />

                <!-- BUS TIMING -->
                <!-- DISPLAY TRUE TIME IF BUS ARRIVAL TIME IS ABOVE 1 MIN -->
                <Label v-if="Math.floor(busdata.services[index].next3.duration_ms / 60000) > -1" class="list-group-item-heading">🕗 {{Math.floor(busdata.services[index].next3.duration_ms / 60000) + ' Mins' }}</Label>
                <!-- SHOWS ARRIVING IF BUS ARRIVAL TIME IS UNDER 1 MIN -->
                <Label v-else text="🕗 Arriving" class="list-group-item-heading" />

                <!-- BUS FEATURE -->
                <Label v-if="busdata.services[index].next3.feature == 'WAB'" textWrap="true" class="list-group-item-heading">☑️ Wheelchair</Label>
                <Label v-else textWrap="true" class="list-group-item-heading">❎ Wheelchair </Label>

                <!-- BUS TYPE (SINGLE DECK OR DOUBLE DECK) -->
                <!-- SHOW SINGLE DECK IF busdata.services[index].NEXT3.TYPE IS SD -->
                <Label v-if="busdata.services[index].next3.type == 'SD'" textWrap="true" class="list-group-item-heading">🚌 Single Deck</Label>
                <!-- SHOW DOUBLE DECK IF busdata.services[index].NEXT3.TYPE IS DD -->
                <Label v-if="busdata.services[index].next3.type == 'DD'" textWrap="true" class="list-group-item-heading">🚌 Double Deck</Label>
            </StackLayout>

            <!-- SHOWS LOADING WHILE LOADING BUS STOP DATA -->
            <Label v-else>Loading...</Label>

        </StackLayout>
        <!-- </PullToRefresh> -->



    </Page>
</template>

<script>
    var appconfig = require("../package.json")
    import BusRoutes from './BusRoutes.vue'
    
    export default {

        props: ['busstopnumber', 'index'],

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
                // console.log(this.busstopnumber)
                console.log(this.index)
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
            },

            navigateToBusRoute(){
                this.$navigateTo(BusRoutes , {
                    props: {
                        'busnumber': this.busdata.services[this.index].no
                    }
                })
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

    .big{
        font-size: 30;
    }
    
</style>
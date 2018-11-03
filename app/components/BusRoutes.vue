<template>
    <Page>
        <ActionBar :title="'Routes For Bus ' + busnumber" class="action-bar" />


        <!-- <PullToRefresh @refresh="refreshList"> -->
        <StackLayout>

            <ListView class="list-group" for="bus in busroute[1].stops" @itemTap="tapBusStopCode" v-if="app.mode == 'view_bus' && loaded.busroute">
                <v-template>

                    <StackLayout class="list-group-item">

                        <!-- DISPLAY BUS NUMBER -->
                        <Label :text="bus" class="list-group-item-heading own-header" />

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
    import BusInfo from './BusInfo.vue'
    import BusTiming from './BusTiming.vue'
    
    export default {

        props: ['busnumber'],

        data() {
            return {

                app: {
                    title: "",
                    version: "",
                    mode: "?"
                },

                busroute: [],

                loaded: {
                    busroute: false,
                },

            }
        },

        methods: {

            getBusRoutes(number) {
                
                console.log(number)

                this.app.mode = "view_bus"
                this.loaded.busroute = false

                fetch("https://busrouter.sg/data/2/bus-services/" + number + ".json")

                    .then(response => response.json())

                    .then(json => {
                        this.busroute = json
                        this.loaded.busroute = true

                    })
            },

            tapBusStopCode(args){
                var index = args.index
                // console.log(args.index)
                console.log(this.busroute[1].stops[args.index])

                this.$navigateTo(BusTiming , {
                    props: {
                        'busstopnumber': this.busroute[1].stops[args.index]
                    }
                })
            }

        },

        mounted() {

            // RUN getBusRoutes WITH THE busnumber PROP to uppercase
            this.getBusRoutes(this.busnumber.toUpperCase())
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
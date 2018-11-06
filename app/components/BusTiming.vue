<template>
    <Page>
        <ActionBar :title="busstopnumber" class="action-bar" />

        <StackLayout>

            <Label v-if='loaded.busstopname' :text="busstopname[0].name" class="list-group-item-heading own-header" />

            <StackLayout>
                <PullToRefresh @refresh="refreshList">
                    <ListView class="list-group" for="bus in busdata.services" @itemTap="tapBus">
                        <v-template>
                            <GridLayout rows='*,*,*,*' columns='*' class="list-group-item">
                                <!-- DISPLAY BUS NUMBER -->
                                <Label row='0' col='0' :text="bus.no" class="list-group-item-heading own-header" />

                                <!-- BUS TIMING -->
                                <!-- DISPLAY TRUE TIME IF BUS ARRIVAL TIME IS ABOVE 1 MIN -->
                                <Label row='1' col='0' v-if="bus.next.duration_ms > 0" class="list-group-item-heading">🕗 {{Math.floor(bus.next.duration_ms / 60000) + ' Mins' }}</Label>
                                <!-- SHOWS ARRIVING IF BUS ARRIVAL TIME IS UNDER 1 MIN -->
                                <Label row='1' col='0' v-else text="🕗 Arriving" class="list-group-item-heading" />

                                <!-- BUS TIMING -->
                                <!-- DISPLAY TRUE TIME IF BUS ARRIVAL TIME IS ABOVE 1 MIN -->
                                <Label row='2' col='0' v-if="bus.next2.duration_ms  > 0" class="list-group-item-heading">🕗 {{Math.floor(bus.next2.duration_ms / 60000) + ' Mins' }}</Label>
                                <!-- SHOWS ARRIVING IF BUS ARRIVAL TIME IS UNDER 1 MIN -->
                                <Label row='2' col='0' v-else text="🕗 Arriving" class="list-group-item-heading" />

                                <!-- BUS TIMING -->
                                <!-- DISPLAY TRUE TIME IF BUS ARRIVAL TIME IS ABOVE 1 MIN -->
                                <Label row='3' col='0' v-if="bus.next3.duration_ms > 0" class="list-group-item-heading">🕗 {{Math.floor(bus.next3.duration_ms / 60000) + ' Mins' }}</Label>
                                <!-- SHOWS ARRIVING IF BUS ARRIVAL TIME IS UNDER 1 MIN -->
                                <Label row='3' col='0' v-else text="🕗 Arriving" class="list-group-item-heading" />
                            </GridLayout>
                        </v-template>
                    </ListView>
                </PullToRefresh>
            </StackLayout>

        </StackLayout>


    </Page>
</template>

<script>
    var appconfig = require("../package.json")
    import BusInfo from './BusInfo.vue'
    import BusInfoVue from './BusInfo.vue';

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
                    busstopname: false,
                },

                busstopname: '',


            }
        },

        methods: {

            tapBus(args) {
                console.log(args.index)

                this.$navigateTo(BusInfo, {
                    props: {
                        'busstopnumber': this.busstopnumber,
                        'index': args.index,
                    }
                })
            },

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

            getBusStopName(busstopnumber) {
                fetch("https://busrouter.sg/data/2/bus-stops.json")
                    .then(res => res.json())
                    .then(json => {
                        this.busstopname = json.filter(data => {
                            return data.no.match(busstopnumber)
                        })

                        this.loaded.busstopname = true;

                    })
            },

            refreshList(args) {


                var pullRefresh = args.object;

                console.log(this.busstopnumber)
                this.getBusStopTiming(this.busstopnumber)

                setTimeout(function () {
                    pullRefresh.refreshing = false;
                }, 1000);
            }


        },

        mounted() {

            // RUN GETBUSSTOPTIMING WITH THE BUSSTOPNUMBER PROP
            this.getBusStopTiming(this.busstopnumber)
            this.getBusStopName(this.busstopnumber)
            console.log('Here:', this.busstopname)
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
<template>
    <Page class="page">
        <ActionBar :title="app.title + ' ' + app.version" class="action-bar" />

            <StackLayout class="home-panel">
                <!--Add your page content here-->

                <Button @tap="getBusStopData()"> Back </Button>
                
                <TextField v-model="input" @textChange="verifyInput()" />
                
                <ListView class="list-group" for="busstop in busstopdata" @itemTap="busstoptap" style="height:1250px"
                    v-if="app.mode == 'search' && loaded.busstopdata">
                    <v-template>
                        <FlexboxLayout flexDirection="row" class="list-group-item">
                            <Label :text="busstop.name" class="list-group-item-heading" style="width: 60%" />
                        </FlexboxLayout>
                    </v-template>
                </ListView>

                <ListView class="list-group" for="bus in busdata.services" @itemTap="busstoptap" style="height:1250px"
                    v-if="app.mode == 'view_bus' && loaded.busdata">
                    <v-template>

                        <StackLayout flexDirection="row" class="list-group-item">
                            <Label :text="bus.no" class="list-group-item-heading" style="width: 60%" />
                            <Label :text="Math.floor(bus.next.duration_ms / 60000) + ' Mins' " class="list-group-item-heading" style="width: 60%" />
                        </StackLayout>

                    </v-template>
                </ListView>

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

                input: ""

            }
        },

        methods: {

            verifyInput(){
                if(this.input == ""){
                    this.getBusStopData()
                }else{
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

    StackLayout{
        color: white;
    }

    ActionBar {
        background-color: #53ba82;
        color: #ffffff;
    }

    .message {
        vertical-align: center;
        text-align: center;
        font-size: 20;
        color: #333333;
    }
</style>
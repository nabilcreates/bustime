<template>
    <Page>
        <ActionBar :title="app.title + ' ' + app.version" class="action-bar" />

        <StackLayout>
            <!--Add your page content here-->

            <TextField v-model="input" hint="Enter a bus stop code" />

            <!-- BUTTON -->
            <Button @tap="verifyInputBusTiming()"> Get Bus Stop Times </Button>
            <Button @tap="verifyInputBusRoutes()"> Get Bus Routes </Button>

            <Label class="or-divider"> OR </Label>

            <Button @tap="navigateToBusStopList()"> Bus Stop List </Button>

            <!-- CLEAR HISTORY -->
            <Button @tap="clearHistory()"> Clear History </Button>

            <!-- HISTORY LISTVIEW -->
            <ListView class="list-group" for="hist in history" @itemTap="tapHistory" style="height:1250px">
                <v-template>
                    <StackLayout flexDirection="row" class="list-group-item">

                        <!-- BUS STOP NAME -->
                        <Label :text="hist.input" class="list-group-item-heading own-header" />
                        <Label :text="hist.type" class="list-group-item-heading" />

                    </StackLayout>
                </v-template>
            </ListView>

        </StackLayout>

    </Page>
</template>

<script>
    var appconfig = require("../package.json")

    import BusStopList from './BusStopList.vue'
    import BusTiming from './BusTiming.vue'
    import BusRoutes from './BusRoutes.vue'

    export default {
        data() {
            return {

                app: {
                    title: "",
                    version: "",
                    mode: "?"
                },

                input: "",

                history: [],

                components: {
                    BusStopList,
                }

            }
        },

        methods: {

            // centeralize(){
            //     var inputstr = this.input.toString()

            //     if(inputstr.length = 5){
            //         this.navigateToBusTiming(this.input)
            //     }else{
            //         this.navigateToBusRoutes(this.input)
            //     }
                
            // },
            
            tapHistory(args) {
                
                switch (this.history[args.index].spid) {
                    case 0:
                        this.navigateToBusTiming(this.history[args.index].input)    
                        break;

                    case 1:
                        this.navigateToBusRoutes(this.history[args.index].input)
                        break;
                    
                    default:
                        break;
                }
                

            },

            clearHistory() {
                this.history = []
            },

            verifyInputBusTiming() {

                console.log("verify")
                console.log(this.input)

                // SPID STANDS FOR SPECIAL ID TO MAKE SURE IF ITS GETTING BUS ROUTE OR GETTING BUS STOP TIMING
                // 0 FOR BUS STOP TIMING
                // 1 FOR BUS ROUTES

                this.history.push({
                    input: this.input,
                    type: "Bus Stop Timing",
                    spid: 0,
                })

                this.navigateToBusTiming(this.input)

            },

            navigateToBusTiming(busstop) {

                console.log("bustiming")
                console.log(busstop)

                this.$navigateTo(BusTiming, {
                    props: {
                        'busstopnumber': busstop,
                    }
                })
            },

            navigateToBusStopList() {
                this.$navigateTo(BusStopList)
            },

            getAppInfo() {
                this.app.title = appconfig.name
                this.app.version = appconfig.version
            },

            verifyInputBusRoutes() {

                // SPID STANDS FOR SPECIAL ID TO MAKE SURE IF ITS GETTING BUS ROUTE OR GETTING BUS STOP TIMING
                // 0 FOR BUS STOP TIMING
                // 1 FOR BUS ROUTES

                this.history.push({
                    input: this.input,
                    type: "Bus Route",
                    spid: 1,
                })

                this.navigateToBusRoutes(this.input)
            },

            navigateToBusRoutes(number) {
                this.$navigateTo(BusRoutes, {
                    props: {
                        'busnumber': number
                    }
                })
            }

        },

        mounted() {
            this.getAppInfo()
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


    .or-divider {
        text-align: center;
    }
</style>
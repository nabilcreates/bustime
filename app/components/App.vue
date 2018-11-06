<template>
    <Page>
        <ActionBar :title="app.title + ' ' + app.version" class="action-bar" />

        <StackLayout>
            <!--Add your page content here-->

            <TextField v-model="input" hint="Enter search term (Bus Stop Name / Bus Stop Code)" />

            <GridLayout columns='*,*,*' rows='*' height='45'>

                <Button col='0' row='0' @tap="navigateToBusStopSearchPage(input)">Bus Stop</Button>
                <Button col='1' row='0' @tap="navigateToBusCodeSearchPage(input)">Stop Code</Button>
                <Button col='2' row='0' @tap="navigateToBusRoutes(input)">Route</Button>

            </GridLayout>
            <Button col='1' row='0' @tap="navigateToBusStopList()"> Bus Stop List </Button>

            <Label textWrap='true'>This is your search history:</Label>
            <ListView class="list-group" for="item in history" @itemTap="tapHistory" style="height:1250px">
                <v-template>
                    <StackLayout class="list-group-item">

                        <!-- HISTORY -->
                        <Label :text="item" class="list-group-item-heading" />

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
    import BusStopSearchVue from './BusStopSearch.vue';
    import BusCodeSearchVue from './BusCodeSearch.vue';

    export default {
        data() {
            return {

                app: {
                    title: "",
                    version: "",
                    mode: "?"
                },

                input: "",

                selectedItem: "",

                components: {
                    BusStopList,
                },

                history: [],

            }
        },

        methods: {

            navigateToBusTiming(busstop) {

                console.log("bustiming")
                console.log(busstop)

                this.$navigateTo(BusTiming, {
                    props: {
                        'busstopnumber': busstop,
                    }
                })
            },

            navigateToBusRoutes(input) {

                // NOT FOUND IN HISTORY ARRAY
                if (this.history.indexOf(input) < 0) {

                    // PUSH IT
                    this.history.push(input)
                }


                this.$navigateTo(BusRoutes, {
                    props: {
                        'busnumber': input
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

            navigateToBusCodeSearchPage(input) {

                // NOT FOUND IN HISTORY ARRAY
                if (this.history.indexOf(input) < 0) {

                    // PUSH IT
                    this.history.push(input)
                }

                this.$navigateTo(BusCodeSearchVue, {
                    props: {
                        searchterm: input,
                    }
                })

            },

            navigateToBusStopSearchPage(input) {

                // NOT FOUND IN HISTORY ARRAY
                if (this.history.indexOf(input) < 0) {

                    // PUSH IT
                    this.history.push(input)
                }

                this.$navigateTo(BusStopSearchVue, {
                    props: {
                        searchterm: input
                    }
                })
            },

            tapHistory(args) {
                this.input = this.history[args.index]
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
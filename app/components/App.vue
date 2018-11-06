<template>
    <Page>
        <ActionBar :title="app.title + ' ' + app.version" class="action-bar" />

        <StackLayout>
            <!--Add your page content here-->

            <TextField v-model="input" hint="Enter" />

            <!-- BUTTON -->


            <Label text='Search:' />
            <GridLayout columns='*,*' rows='*' height='50'>
                <Button col='0' row='0' @tap="navigateToSearchTerm(input)"> By Name </Button>
                <Button col='1' row='0' @tap="navigateToBusCodeSearch(input)"> By Code</Button>
            </GridLayout>

            <Label text='Others:' />
            <Button col='2' row='1' @tap="navigateToBusStopList()"> Stop List </Button>

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

                components: {
                    BusStopList,
                }

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

            navigateToBusRoutes(number) {
                this.$navigateTo(BusRoutes, {
                    props: {
                        'busnumber': number
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


            navigateToSearchTerm(input) {

                console.log(input)

                this.$navigateTo(BusStopSearchVue, {
                    props: {
                        searchterm: input,
                    }
                })
            },

            navigateToBusCodeSearch(input) {
                this.$navigateTo(BusCodeSearchVue, {
                    props: {
                        searchterm: input,
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
<template>
    <Page>
        <ActionBar :title="app.title + ' ' + app.version" class="action-bar" />

        <StackLayout>
            <!--Add your page content here-->

            <TextField v-model="input" hint="Enter" />

            <ListPicker :items="['Bus Stop' , 'Stop Code']" v-model="selectedItem" />
            
            <GridLayout columns='*,*' rows='*' height='45'>

                <Button col='0' row='0' @tap="navigateToSearchPage(input)">Search </Button>
                <!-- BUTTON -->
                <Button col='1' row='0' @tap="navigateToBusStopList()"> Bus Stop List </Button>

            </GridLayout>


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


            navigateToSearchPage(input) {

                console.log(this.selectedItem)

                // IF THE SELECTED ITEM INDEX IS 0 [BUS STOP]
                if(this.selectedItem == 0){
                    this.$navigateTo(BusStopSearchVue , {
                        props: {
                            searchterm: input
                        }
                    })
                }else{
                    this.$navigateTo(BusCodeSearchVue, {
                        props: {
                            searchterm: input,
                        }
                    })

                }

                this.input = ''
                this.history.push(input)
                
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
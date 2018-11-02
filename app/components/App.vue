<template>
    <Page>
        <ActionBar :title="app.title + ' ' + app.version" class="action-bar" />

        <StackLayout>
            <!--Add your page content here-->

            <TextField v-model="input" hint="Enter a bus stop code" />

            <!-- BUTTON -->
            <Button @tap="navigateToBusTiming(input)"> Get Bus Times </Button>

            <Label class="or-divider"> OR </Label>
            
            <Button @tap="navigateToBusStopList()"> Bus Stop List </Button>

        </StackLayout>

    </Page>
</template>

<script>
    var appconfig = require("../package.json")

    import BusStopList from './BusStopList.vue'
    import BusTiming from './BusTiming.vue'

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
                this.$navigateTo(BusTiming, {
                    props: {
                        'busstopnumber': busstop,
                    }
                })
            },

            navigateToBusStopList(){
                this.$navigateTo(BusStopList)
            },
            
            getAppInfo() {
                this.app.title = appconfig.name
                this.app.version = appconfig.version
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


    .or-divider{
        text-align: center;
    }
    
</style>
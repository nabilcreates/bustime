<template>
    <Page>
        <ActionBar title="Bus Stop List" class="action-bar" />

        <StackLayout>

            <ListView class="list-group" for="busstop in busstopdata" @itemTap="busstoptap" style="height:1250px" v-if="app.mode == 'search' && loaded.busstopdata">
                <v-template>
                    <StackLayout flexDirection="row" class="list-group-item">

                        <!-- BUS STOP NAME -->
                        <Label :text="busstop.name" class="list-group-item-heading own-header" />

                        <!-- BUS STOP NUMBER -->
                        <Label :text="busstop.no" class="list-group-item-heading" />

                    </StackLayout>
                </v-template>
            </ListView>

        </StackLayout>

    </Page>
</template>
</template>

<script>
    import BusTiming from "./BusTiming.vue"

    export default {

        data() {

            return {
                app: {
                    title: "",
                    version: "",
                    mode: "?"
                },

                busstopdata: [],

                loaded: {
                    busstopdata: false,
                },

            }

        },

        methods: {

            busstoptap(args) {
                var busstopnum = this.busstopdata[args.index].no
                var busstopname = this.busstopdata[args.index].name

                this.$navigateTo(BusTiming,

                    {
                        props: {
                            'busstopnumber': busstopnum,
                            'busstopname': busstopname,
                        }
                    }

                )
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
        },

        mounted() {
            this.getBusStopData()
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
<template>
    <Page>
        <ActionBar title="Search (By Name)" class="action-bar" />

        <StackLayout>

            <ListView class="list-group" for="busstop in searchFilter" @itemTap="busstoptap" style="height:1250px" v-if="loaded.busstopdata && !searchFilter == []">
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

<script>
    import BusTiming from "./BusTiming.vue"

    export default {

        props: ['searchterm'],
        
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

                searchFilter: [],

            }

        },

        methods: {

            busstoptap(args) {
                this.$navigateTo(BusTiming,

                    {
                        props: {
                            'busstopnumber': this.searchFilter[args.index].no,
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

                        this.getSearchResult()
                        
                    })
            },

            getSearchResult(){
                this.searchFilter = this.busstopdata.filter(data => {
                    return data.name.toLowerCase().match(this.searchterm)
                })
            }
            
        },

        mounted() {
            this.getBusStopData()
            console.log(this.searchFilter)
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
<template>
    <Page>
        <ActionBar title="Search" class="action-bar" />

        <StackLayout>

            <TabView>
                <TabViewItem title="Name">
                    <BusStopSearch :searchterm='searchterm' />
                </TabViewItem>
                <TabViewItem title="Code">
                    <BusCodeSearch :searchterm='searchterm' />
                </TabViewItem>
            </TabView>

        </StackLayout>

    </Page>
</template>

<script>
import BusCodeSearch from './BusCodeSearch.vue';
import BusStopSearch from './BusStopSearch.vue';

    export default {

        props: ['searchterm'],

        components: {
            BusStopSearch,
            BusCodeSearch
            
        },

    }
</script>

<style scoped>
    /* PCOLOR: #e24747 */

    .list-group-item-heading {
        color: #161616;
    }

    /* StackLayout {
        margin: 0 10;

    } */

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
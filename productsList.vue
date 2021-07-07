<template>
    <ListView for="product in products" @itemTap="onItemTap"
        style="color:white; height:100%">
        <v-template>
            <DockLayout height="500" stretchLastChild="false">
                <Image dock="top" height="200"
                    :src="`https://hmodu.github.io/Course-work-2/Client/${product.image}`"
                    height="50%" width="100%" />
                <Label
                    :text="`Available Inventory: ${product.availableInventory}`"
                    dock="bottom" width="100%" />
                <Label :text="`price: $${product.price}`" dock="bottom"
                    width="100%" />
                <Label :text="`location: ${product.location}`" dock="bottom"
                    width="100%" />
                <Label :text="product.subject" dock="bottom" />
            </DockLayout>
        </v-template>
    </ListView>
</template>

<script>
    export default {
        props: ["products"],
        data() {
            return {};
        },
        methods: {
            onItemTap(event) {
                this.$emit("addProduct", event.item);
            }
        }
    };
</script>

<style>
</style>
<template>
    <StackLayout>
        <Label class="h1 text-center" text="Checkout" style="color:white" />
        <Label class="h5 text-center" text="tap lesson to remove it cart"
            style="color:green" />
        <ListView for="product in cart" @itemTap="onItemTap"
            style="color:white; height:50%">
            <v-template>
                <StackLayout>
                    <Label :text="product.subject" />
                    <Label :text="`price: ${product.price}`" />
                    <Label
                        :text="`inventory: ${product.availableInventory}`" />
                </StackLayout>
            </v-template>
        </ListView>
        <TextField style="color:white;" hint=" name"
            v-model="checkout.name" />
        <TextField style="color:black;" hint=" phone number"
            v-model="checkout.phone" />
        <Button @tap="submit" text="Submit Order" />
    </StackLayout>
</template>

<script>
    export default {
        props: {
            cart: {
                type: Array
            },
            products: {
                type: Array
            }
        },
        data() {
            return {
                checkout: {
                    name: "",
                    phone: ""
                }
            };
        },

        methods: {
            onItemTap(event) {
                this.$emit("removeProduct", event.item);
            },
            submitForm() {
                fetch("https://moscst3145.herokuapp.com/collection/orders", {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        Name: this.order.name,
                        Phone: this.order.phone,
                        ProductsInOrders: this.cart.map(({
                            subject
                        }) => subject)
                    })
                }).then(function() {});
                this.cart.forEach(element => {
                    fetch(
                        "https://moscst3145.herokuapp.com/collection/products/" +
                        element._id, {
                            method: "PUT",
                            headers: {
                                "Content-Type": "application/json"
                            },
                            body: JSON.stringify({
                                availableInventory: element
                                    .availableInventory
                            })
                        }
                    ).then(function(data) {});
                });
                alert("Order submitted");
            },
            cartCount(id) {
                let count = 0;
                for (let i = 0; i < this.cart.length; i++) {
                    if (this.cartID[i] === id) {
                        count++;
                    }
                }
                return count;
            }
        }
    };
</script>
<style>
</style>

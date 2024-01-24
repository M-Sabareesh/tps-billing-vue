<template>
    <div class="columns">
        <div class="column is-3">
            <div class="field">
                <label>Title</label>
                <div class="control">
                    <input type="text" class="input" v-model="item.title">
                </div>
            </div>
        </div>

        <div class="column is-3">
            <div class="field">
                <label>Price per gram</label>
                <div class="control">
                    <input type="number" class="input" v-model="item.price_per_gram">
                </div>
            </div>
        </div>
        
        <div class="column is-3">
            <div class="field">
                <label>Item Weight</label>
                <div class="control">
                    <input type="number" class="input" v-model="item.item_weight">
                </div>
            </div>
        </div>

        <div class="column is-3">
            <div class="field">
                <label>VAT Rate</label>
                <div class="control">
                    <input type="number" class="input" v-model="item.vat_rate">
                </div>
            </div>
        </div>

        <div class="column is-2">
            <div class="field">
                <label>Gross Amount</label>
                <div class="control">
                    <input type="text" class="input" v-bind:value="gross_amount" disabled>
                </div>
            </div>
        </div>

        <div class="column is-3">
            <div class="field">
                <div class="control">
                    <button class="button is-danger is-small" @click="removeItem">-</button>
                </div>
            </div>
        </div>
        
    </div>
    
</template>

<script>
export default {
    name: 'ItemForm',
    props: {
        initialItem: Object
    },
    data() {
        return {
            item: this.initialItem,
        }
    },
    computed: {
        gross_amount() {
            const price_per_gram = this.item.price_per_gram
            const item_weight = this.item.item_weight
            const vat_rate = this.item.vat_rate
            this.item.net_amount = price_per_gram * item_weight

            this.$emit('updatePrice', this.item)
            return this.item.net_amount + ( this.item.net_amount * (vat_rate / 100))
        },
        
    },
    methods: {
        removeItem() {
            this.$emit('remove-item', this.item);
        },
    }

}
</script>

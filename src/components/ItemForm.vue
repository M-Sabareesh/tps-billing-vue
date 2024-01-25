<template>
    <div class="columns">
        <div class="column is-1.5" style="flex: 0 0 12.5%;">
            <div class="field">
                <label>Metal</label>
                <div class="control">
                    <div class="select">
                        <select v-model="item.metal">
                                <option value="Gold">Gold</option>
                                <option value="Silver">Silver</option>
                        </select>
                    </div>
                </div>
            </div>
        </div>

        <div class="column is-1.5" style="flex: 0 0 12.5%;">
            <div class="field">
                <label>Jewel</label>
                <div class="control">
                    <div class="select">
                        <select v-model="item.jewel">
                                <option value="Ring">Ring</option>
                                <option value="Bangle">Bangle</option>
                                <option value="Ear ring">Ear rings</option>
                                <option value="Nose Ring">Nose ring</option>
                                <option value="Chain">Chain</option>
                                <option value="Bracelet">Bracelet</option>
                        </select>
                    </div>
                </div>
            </div>
        </div>

        <div class="column is-1.5" style="flex: 0 0 12.5%;">
            <div class="field">
                <label>Price/gram</label>
                <div class="control">
                    <input type="number" class="input" v-model="item.price_per_gram">
                </div>
            </div>
        </div>
        
        <div class="column is-1.5" style="flex: 0 0 12.5%;">
            <div class="field">
                <label>Item Wt.</label>
                <div class="control">
                    <input type="number" class="input" v-model="item.item_weight">
                </div>
            </div>
        </div>

        <div class="column is-1.5" style="flex: 0 0 12.5%;">
            <div class="field">
                <label>Stone Wt.</label>
                <div class="control">
                    <input type="number" class="input" v-model="item.stone_weight">
                </div>
            </div>
        </div>

        <div class="column is-1.5" style="flex: 0 0 12.5%;">
            <div class="field">
                <label>Net Wt.</label>
                <div class="control">
                    <input type="number" class="input" v-bind:value="net_weight" disabled>
                </div>
            </div>
        </div>

        <div class="column is-1.5" style="flex: 0 0 12.5%;">
            <div class="field">
                <label>Stone Price</label>
                <div class="control">
                    <input type="number" class="input" v-model="item.stone_price">
                </div>
            </div>
        </div>

        <div class="column is-2">
            <div class="field">
                <label>Making Charges</label>
                <div class="control">
                    <input type="number" class="input" v-model="item.making_charges">
                </div>
            </div>
        </div>

        <div class="column is-1.5" style="flex: 0 0 12.5%;">
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

        <div class="column is-1">
            <div class="field">
                <div class="control">
                    <br/>
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
            const net_weight = parseFloat(this.item.item_weight - this.item.stone_weight).toFixed(2)
            const vat_rate = this.item.vat_rate
            //this.item.net_amount = price_per_gram * net_weight
            if (isNaN(price_per_gram) || isNaN(net_weight) || isNaN(vat_rate)) {
                return ''; // or return 0 or any other default value
            }
            this.item.net_amount = (price_per_gram * net_weight) + this.item.stone_price 
            this.$emit('updatePrice', this.item)
            //return this.item.net_amount + ( this.item.net_amount * (vat_rate / 100))
            //return parseFloat(item_net_pri150ce + ( item_net_price * (vat_rate / 100))).toFixed(2)
            return parseFloat(this.item.net_amount + this.item.making_charges + ( this.item.net_amount * (vat_rate / 100))).toFixed(2)
        },
        net_weight() {
            return parseFloat(this.item.item_weight - this.item.stone_weight).toFixed(2);
        }
        
    },
    methods: {
        removeItem() {
            this.$emit('remove-item', this.item);
        },
    }

}
</script>

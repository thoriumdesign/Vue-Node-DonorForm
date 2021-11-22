<template>
  <div>
    <v-row>
      <v-col>
        <v-chip-group
          active-class="primary--text"
          column
          @change="setPrice"
        >
          <v-chip
            v-for="(amount, index) in amounts" 
            v-bind:key="index"
          >
            {{ currencyFormat(amount) }}
          </v-chip>
          <v-chip v-if="allow_custom">Custom Amount</v-chip>
        </v-chip-group>
      </v-col>
    </v-row>
    <v-row v-show="custom">
        <v-col>
          <v-text-field
            label="Custom Amount"
            type="number"
            @input="setCustom"
          ></v-text-field>
        </v-col>
      </v-row>
  </div>
</template>

<script>
export default {
  props: ["amounts", "allow_custom"],
  data() {
    return {
      custom: false,
    }
  },
  methods: {
    returnPrice(new_price) {
      this.$emit("change", new_price)
    },

    setPrice(chip_index) {
      if (chip_index >= this.amounts.length) return this.custom = true
      this.custom = false
      console.log(this.amounts[chip_index])
      this.returnPrice(this.amounts[chip_index])
    },

    setCustom(new_price) {
      this.returnPrice(new_price)
    },

    currencyFormat(value) {
      if (!value) return ''
      const currency_string = Number(value).toLocaleString('en', {
        style: 'currency', currency: 'USD'
      })
      if (value % 1 === 0) return currency_string.slice(0, -3)
      return currency_string
    },
  },
}
</script>